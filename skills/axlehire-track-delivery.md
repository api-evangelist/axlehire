---
generated: '2026-08-06'
method: generated
name: Track a delivery end to end
description: Follow a Jitsu shipment from creation to proof of delivery using webhooks first, tracking events second, and status polling only as a fallback.
api: openapi/axlehire-jitsu-rest-api.yml
operations: [retrieve_1, retrieveEvents, retrieveHistory_1, retrieveHistory, getPod]
source: >-
  operationIds verified verbatim in openapi/axlehire-jitsu-rest-api.yml; event
  catalog from asyncapi/axlehire-webhooks.yml; lifecycle states from
  lifecycle/axlehire-lifecycle.yml.
---

# Track a delivery end to end

## Auth
`Authorization: Token <YOUR_API_TOKEN>`. See `authentication/axlehire-authentication.yml`.

## Prefer webhooks. Poll only as a fallback.
Jitsu pushes 33 distinct event types across five categories (Planning, Inbound, Outbound, POD, Exceptions). Registration is manual — email your endpoint URL and any auth requirement to `api@gojitsu.com`; there is no subscription API. Full catalog in `asyncapi/axlehire-webhooks.yml`.

Envelope: `{event, ts, geolocation, data}`. `data.shipment` carries `{id, shipment_id, internal_id, tracking_code, assignment_id}`.

**Your handler must be idempotent** — delivery is at-least-once. Deduplicate on `ts` + `event` + `data.shipment.id`. Return 2xx fast and process asynchronously; a non-2xx makes Jitsu retry with backoff.

## Steps
1. **Current status** — `retrieve_1` (`GET /v3/shipments/{shipment_id}`). Read `status` (delivery lifecycle) **and** `inbound_status` (warehouse handoff) — they are two independent state machines.
2. **Full event history by tracking code** — `retrieveEvents` (`GET /v3/tracking/{tracking_code}/events`). Each `TrackingEvent` has `signal`, `ts`, `geolocation`, `eta` (milliseconds), `remark`, `next_destination`, `type`.
3. **Tracking summary** — `retrieveHistory_1` (`GET /v3/tracking/{tracking_code}`). Shipment-scoped history is `retrieveHistory` (`GET /v3/shipments/{shipment_id}/history`).
4. **Proof of delivery** — `getPod` (`GET /v3/shipments/{shipment_id}/pod`) returns `DeliveryPod` entries `{url, category, capture_ts}`. POD **webhooks** (`POD.ACCEPTED` / `POD.NEED_REVIEW` / `POD.REJECTED`) are **not enabled by default** — ask Jitsu to turn them on for your account.
5. **Recipient-facing tracking** — the shipment's `tracking_url` is `https://recipient.gojitsu.com/{tracking_code}`. Surface it in your own confirmation email or order page rather than rebuilding the map. See `components/axlehire-components.yml`.

## Terminal states to handle
- Success: `DROPOFF_SUCCEEDED`
- Failure: `DROPOFF_FAILED`, then either re-delivery or `RETURN_*` (`RETURN_SUCCEEDED`, `RETURN_FAILED`, `RETURN_DAMAGED`, `RETURNED_TO_CLIENT_SUCCEEDED`)
- Never routed: `GEOCODE_FAILED`, `UNSERVICEABLE`
- Written off: `DAMAGED`, `DISPOSABLE`
Full list in `lifecycle/axlehire-lifecycle.yml`.

## Address-correction events need special handling
`SHIPMENT.NOTIFICATION` is emitted for all three DAS (Deliverable Address Service) outcomes and is discriminated only by `data.event_data.update_status`: `CORRECTED`, `FLAGGED` or `FAILED`. `FLAGGED` and `FAILED` mean Jitsu has SMS'd the recipient for confirmation. The payload carries `tracking_update_url`, a personalized Manage Delivery link.

## If you miss events
Jitsu retries with backoff, but an endpoint down for long enough will drop events. Recover by polling `retrieve_1` for the affected shipments and `retrieveEvents` for full history, then ask Jitsu to review delivery logs.

## Rate limits
Polling competes with everything else against one account-wide **10 QPS** budget, and there are no rate-limit headers to pace against. Poll on demand (a customer opening an order page), not on a timer. See `rate-limits/axlehire-rate-limits.yml`.
