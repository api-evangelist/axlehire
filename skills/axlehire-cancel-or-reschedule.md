---
generated: '2026-08-06'
method: generated
name: Cancel or reschedule a shipment
description: Cancel a Jitsu shipment before or after pickup, or change its delivery window, dropoff address or delivery instructions while it is still changeable.
api: openapi/axlehire-jitsu-rest-api.yml
operations: [retrieve_1, cancelShipment, updateShipmentTimeWindow, updateDropoffAddress, updateShipmentDropoffLocationInfo, notes]
source: >-
  operationIds verified verbatim in openapi/axlehire-jitsu-rest-api.yml; timing
  and status semantics from https://docs.gojitsu.com/#/docs/FAQs.md and
  https://docs.gojitsu.com/#/docs/Lifecycle.md.
---

# Cancel or reschedule a shipment

## Auth
`Authorization: Token <YOUR_API_TOKEN>`. See `authentication/axlehire-authentication.yml`.

## Check state first
Call `retrieve_1` (`GET /v3/shipments/{shipment_id}`) and read `status` before mutating. What you can change depends on where the shipment is in `lifecycle/axlehire-lifecycle.yml` — most edits are only accepted before pickup.

## Cancel
`cancelShipment` (`POST /v3/shipments/{shipment_id}/cancel`).
The resulting status depends on timing, and Jitsu decides it — not you:
- `CANCELLED_BEFORE_PICKUP` — cancelled before the driver picked it up.
- `CANCELLED_AFTER_PICKUP` — cancelled after pickup, before delivery.

Both are emitted as webhooks. Cancellation is **irreversible via the API** — there is no un-cancel operation. Confirm the resulting status rather than assuming which one you got.

## Reschedule the delivery window
`updateShipmentTimeWindow` (`POST /v3/shipments/{shipment_id}/dropoff-time`) with a `TimeWindow` — `dropoff_earliest_ts` / `dropoff_latest_ts`, **ISO-8601 UTC**. The window is the delivery promise to the recipient, and Jitsu routes against it.

## Change where it goes
- `updateDropoffAddress` (`PATCH /v3/shipments/{shipment_id}/dropoff_address`) — a new `Address`. Expect re-geocoding; watch for `GEOCODE_FAILED` or `UNSERVICEABLE` afterwards.
- `updateShipmentDropoffLocationInfo` (`PUT /v3/shipments/{shipment_id}/dropoff-location-info`) — access codes and additional delivery instructions (`dropoff_access_code`, `dropoff_additional_instruction`).
- `notes` (`PATCH /v3/shipments/{shipment_id}/notes`) — pickup/dropoff/return remarks for the driver.
- `updatePickupAddress` (`PATCH /v3/shipments/{shipment_id}/pickup_address`) and `updateCustomer` (`PATCH /v3/shipments/{shipment_id}/customer`) cover the origin and the recipient record.

## Address corrections may already be in flight
Jitsu's Deliverable Address Service can correct an address on its own and SMS the recipient. If you receive a `SHIPMENT.NOTIFICATION` webhook with `data.event_data.update_status` of `CORRECTED`, `FLAGGED` or `FAILED`, reconcile with your own record before pushing a competing address update. See `asyncapi/axlehire-webhooks.yml`.

## Errors
No idempotency key exists on any of these operations, so a retried cancel or window change after a timeout re-issues the mutation. Read state back with `retrieve_1` instead of blind-retrying. `412`/`422` here usually mean the shipment has moved past the point where the change is accepted — the `message` field says which. See `errors/axlehire-problem-types.yml` and `conventions/axlehire-conventions.yml`.
