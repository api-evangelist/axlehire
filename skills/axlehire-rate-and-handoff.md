---
generated: '2026-08-06'
method: generated
name: Rate a delivery and mark it shipped
description: Get an estimated delivery fee before committing, read the actual fee after, and tell Jitsu when you have physically handed shipments off.
api: openapi/axlehire-jitsu-rest-api.yml
operations: [getEstimatedFee, getShipmentDeliveryFee, markAsShipped, markAsShippedBulk, readyShipment]
source: >-
  operationIds verified verbatim in openapi/axlehire-jitsu-rest-api.yml;
  inbound_status semantics from https://docs.gojitsu.com/#/docs/Glossary.md and
  https://docs.gojitsu.com/#/docs/FAQs.md.
---

# Rate a delivery and mark it shipped

## Auth
`Authorization: Token <YOUR_API_TOKEN>`. See `authentication/axlehire-authentication.yml`.

## Rate before you commit
`getEstimatedFee` (`POST /v3/shipments/rating`) returns an estimate for a prospective shipment without creating one. Use it to price a delivery option at checkout or to compare against another carrier before calling `create`.

After the shipment exists, `getShipmentDeliveryFee` (`GET /v3/shipments/{shipment_id}/fee`) returns the actual `Price` — `unit`, `base`, `pre_delivery_adjustment`, `post_delivery_adjustment`. The two adjustment fields are why the estimate and the final fee can differ; reconcile on the post-delivery value, not the estimate.

## Tell Jitsu the box is on its way
`inbound_status` is a second, independent state machine from `status`. It tracks whether the shipment has physically reached Jitsu.

- `markAsShipped` (`POST /v3/shipments/{shipment_id}/mark-shipped`) — one shipment.
- `markAsShippedBulk` (`POST /v3/shipments/mark-shipped`) — a batch. Prefer this: you have one account-wide **10 QPS** budget, so batching is the difference between one call and a thousand (`rate-limits/axlehire-rate-limits.yml`).

Both set `inbound_status` to `SHIPPED`. Confirm with `retrieve_1` (`GET /v3/shipments/{shipment_id}`) — the docs make this an explicit go-live checklist item.

## Warehouse-side outcomes you must handle
Once Jitsu receives it, the inbound scan status becomes one of:
- `RECEIVED_OK` — at the facility, ready for delivery
- `RECEIVED_DAMAGED` — arrived damaged
- `MISSING` — never arrived at the expected facility

`SHIPMENT.MANIFEST_NOT_RECEIVED` also fires when a shipment is not scanned in within the configured threshold window. These are inventory-loss signals, not delivery signals — route them to operations, not to the customer-notification path. See `asyncapi/axlehire-webhooks.yml`.

## Marking a shipment ready
`readyShipment` (`POST /v3/shipments/{shipment_id}/ready`) signals a shipment is ready for its next stage. Use it where your fulfillment flow decouples creation from readiness.

## Errors and retries
None of these operations are idempotent — there is no `Idempotency-Key`. A retried `markAsShippedBulk` after a timeout re-submits the whole batch. Read back with `retrieve_1` before retrying. See `conventions/axlehire-conventions.yml` and `errors/axlehire-problem-types.yml`.
