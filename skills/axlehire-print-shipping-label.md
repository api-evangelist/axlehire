---
generated: '2026-08-06'
method: generated
name: Retrieve and print a shipping label
description: Pull a Jitsu shipping label in ZPL, PNG or PDF for a shipment or an individual parcel, and get it signed off for Jitsu's scanners.
api: openapi/axlehire-jitsu-rest-api.yml
operations: [create, label, labelParcel, labelParcels]
source: >-
  operationIds verified verbatim in openapi/axlehire-jitsu-rest-api.yml; formats
  and the base64 response encoding from
  https://docs.gojitsu.com/#/docs/Labels.md.
---

# Retrieve and print a shipping label

## Auth
`Authorization: Token <YOUR_API_TOKEN>`. See `authentication/axlehire-authentication.yml`.

## Steps
1. **Create the shipment** — `create` (`POST /v3/shipments`). Capture the `id`.
2. **Retrieve the label** — `label` (`GET /v3/shipments/{shipment_id}/label`).
   - `format` query parameter: `PDF` (default), `PNG`, or `ZPL`.
   - The response carries a `label` field containing **base64-encoded** label data. Decode before writing to disk or sending to a printer.
3. **Per-parcel labels** — for multi-parcel shipments use `labelParcel` (`GET /v3/shipments/{shipment_id}/parcels/{jitsu_parcel_id}/label`) for a single parcel, or `labelParcels` (`GET /v3/shipments/{shipment_id}/parcels/label`) for all of them.

## Format choice
| Format | Use |
|---|---|
| `ZPL` | Direct-to-thermal Zebra printers |
| `PNG` | Embedding in a UI or an email |
| `PDF` | Desktop printing or archival (default) |

## You may print your own
Jitsu labels are optional — skip the endpoint entirely if you generate your own. But the barcode format must be compatible with Jitsu's scanning hardware, and Jitsu requires a scanner-compatibility sign-off during staging either way.

## Go-live checklist (Jitsu requires this)
- Retrieve a label in every format you use.
- Print one and confirm every field is legible.
- Physically scan all barcodes and QR codes.
- Send a sample staging label to the Jitsu team for scanner sign-off — this is a gate on production credentials. See `lifecycle/axlehire-lifecycle.yml`.

## Errors
Label generation is slower than a normal read; the docs recommend a longer read timeout on these calls (30s baseline, more for labels). Standard status handling in `errors/axlehire-problem-types.yml`; the 10 QPS account limit applies to label calls too (`rate-limits/axlehire-rate-limits.yml`).
