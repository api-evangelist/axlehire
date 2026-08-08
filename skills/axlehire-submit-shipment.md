---
generated: '2026-08-06'
method: generated
name: Submit a shipment for last-mile delivery
description: Create a Jitsu shipment with pickup and dropoff addresses, a delivery window and parcels, then confirm it was accepted.
api: openapi/axlehire-jitsu-rest-api.yml
operations: [create, retrieve_1]
source: >-
  operationIds verified verbatim in openapi/axlehire-jitsu-rest-api.yml; field
  semantics from https://docs.gojitsu.com/#/docs/QuickStart.md and
  https://docs.gojitsu.com/#/docs/FAQs.md.
---

# Submit a shipment for last-mile delivery

`POST /v3/shipments` is the only required call in a Jitsu integration. Everything else is optional.

## Auth
- `Authorization: Token <YOUR_API_TOKEN>` on every request. Tokens are per-environment and carry full account permissions — there are no scopes. See `authentication/axlehire-authentication.yml`.
- Base URL: `https://api.staging.gojitsu.com` while developing, `https://api.gojitsu.com` in production. Production credentials are issued only after Jitsu certifies the integration (`lifecycle/axlehire-lifecycle.yml`).

## Before you retry — this API is NOT idempotent
Jitsu ships no `Idempotency-Key` header. A retried create after a timeout can produce a **second physical delivery**.
- Always send a stable `internal_id` (your own reference) and, if you generate them, a stable `tracking_code`.
- If a create times out, do **not** re-POST. Look the shipment up first. See `conventions/axlehire-conventions.yml`.

## Steps
1. **Create the shipment** — `create` (`POST /v3/shipments`). Required shape:
   - `customer`: `{name, email, phone_number}`
   - `pickup_address` and `dropoff_address`: `{street, street2, city, state, zipcode}`
   - `dropoff_earliest_ts` / `dropoff_latest_ts`: **ISO-8601 UTC**. This is the delivery window to the end recipient. For `NEXT_DAY` service the window must fall on the day *after* the shipment arrives at the Jitsu warehouse.
   - `parcels[]`: each with `dimensions` `{unit, width, length, height}` and `weight` `{unit, value}`
   - Optional: `internal_id`, `tracking_code` (auto-generated if omitted — Jitsu recommends omitting it), `service_level`, `rdi` (`RESIDENTIAL`/`COMMERCIAL`), `sms_enabled`, `signature_required`, `delivery_proof_photo_required`, `id_required`, `code` (brand code), `tags[]`, `extra` (free-form metadata).
   - Response: `{id, tracking_code}` — capture both. `id` is what every other path takes; `tracking_code` is what the tracking endpoints take.
2. **Confirm acceptance** — `retrieve_1` (`GET /v3/shipments/{shipment_id}`). Read `status` and `tracking_url`.

## What "accepted" does not mean
A 2xx on create does not mean the shipment is deliverable. Two outcomes must be handled:
- `GEOCODE_FAILED` — the address could not be geocoded and needs correction before routing.
- `UNSERVICEABLE` — the destination is outside Jitsu's coverage (23 of the 25 largest US metros; confirm ZIPs with Jitsu before go-live).

Both arrive as webhooks and as the shipment `status`. Full state list in `lifecycle/axlehire-lifecycle.yml`.

## Errors
The envelope is `{"message": "..."}` with no error code — HTTP status is the only machine-readable signal, and the OpenAPI declares no 4xx at all. Do not retry 400/401/403/404/406/412/422; retry 429/500/502/503 with exponential backoff (1s→60s, jittered). See `errors/axlehire-problem-types.yml` and `rate-limits/axlehire-rate-limits.yml` (10 QPS, account-wide, no rate-limit headers).

## Notes
- There is no `GET /v3/shipments` list endpoint. If you lose the `id`, you cannot enumerate your own shipments by API — store `id` and `tracking_code` on your side at creation time.
- Test the whole path first: staging has a lifecycle simulation API that drives a shipment to delivery without a real driver. See `sandbox/axlehire-sandbox.yml`.
