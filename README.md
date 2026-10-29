# AxleHire (Jitsu)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AxleHire is a US last-mile delivery carrier founded in 2015 that rebranded as Jitsu in April 2024 and now operates at gojitsu.com. It runs an asset-light, gig-driver network with a proprietary routing and tracking platform serving e-commerce, meal-kit and subscription-box shippers across 23 of the 25 largest US metros, seven days a week. The Jitsu REST API (v3) lets shippers submit shipments, retrieve ZPL/PNG/PDF labels, manage parcels, cancel and re-window deliveries, pull rating estimates and proof-of-delivery, and read the full tracking event history. A documented webhook surface pushes ~33 lifecycle events — planning, inbound scan, outbound pickup/dropoff, proof-of-delivery and exceptions — to a registered endpoint. Jitsu publishes an OpenAPI 3.0.1 contract, nine first-party SDKs, a staging environment with a lifecycle simulation API, and a public status page.

**Areas:** Company · Logistics · Last Mile Delivery · Shipping · Parcel · Transportation · Supply Chain · E-Commerce · Tracking · Webhook

## AxleHire (Jitsu) on APIs.io

Everything profiled for AxleHire (Jitsu) is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/axlehire/](https://apis.io/providers/axlehire/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [AxleHire (Jitsu) Assignment Information API](https://apis.io/apis/axlehire/axlehire-assignment-information-api/) — Assignment specific calls
- [AxleHire (Jitsu) Driver Information API](https://apis.io/apis/axlehire/axlehire-driver-information-api/) — Driver specific calls
- [AxleHire (Jitsu) Partner Information API](https://apis.io/apis/axlehire/axlehire-partner-information-api/) — The Partner Information API from AxleHire (Jitsu) — 1 operation(s) for partner information.
- [AxleHire (Jitsu) Shipping Information API](https://apis.io/apis/axlehire/axlehire-shipping-information-api/) — Shipment specific calls
- [AxleHire (Jitsu) Tracking Information API](https://apis.io/apis/axlehire/axlehire-tracking-information-api/) — Tracking specific calls

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Axlehire Webhooks](https://apis.io/asyncapis/axlehire/axlehire-webhooks/)

#### Examples (4)

Example requests and responses.

- [Axlehire Retrieve Shipment Example](https://apis.io/examples/axlehire/axlehire-retrieve-shipment-example/)
- [Axlehire Shipment Label Example](https://apis.io/examples/axlehire/axlehire-shipment-label-example/)
- [Axlehire Submit Shipment Example](https://apis.io/examples/axlehire/axlehire-submit-shipment-example/)
- [Axlehire Tracking Events Example](https://apis.io/examples/axlehire/axlehire-tracking-events-example/)

#### Collections (5)

Postman and Open Collection request collections.

- [Jitsu REST Assignment Information API](https://apis.io/collections/axlehire/open-axlehire-assignment-information-api/)
- [Jitsu REST Driver Information API](https://apis.io/collections/axlehire/open-axlehire-driver-information-api/)
- [Jitsu REST Partner Information API](https://apis.io/collections/axlehire/open-axlehire-partner-information-api/)
- [Jitsu REST Shipping Information API](https://apis.io/collections/axlehire/open-axlehire-shipping-information-api/)
- [Jitsu REST Tracking Information API](https://apis.io/collections/axlehire/open-axlehire-tracking-information-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Axlehire Jitsu Rest Api Overlay](https://apis.io/overlays/axlehire/axlehire-jitsu-rest-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Axlehire Authentication](https://apis.io/security/axlehire/axlehire-authentication/)
- [Axlehire Domain Security](https://apis.io/security/axlehire/axlehire-domain-security/)
- [Axlehire Trust Center](https://apis.io/security/axlehire/axlehire-trust-center/)
- [Axlehire Vulnerability Disclosure](https://apis.io/security/axlehire/axlehire-vulnerability-disclosure/)

#### Rate limits (1)

Published request limits.

- [Axlehire Rate Limits](https://apis.io/rate-limits/axlehire/axlehire-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://gojitsu.com/)
- [Documentation](https://docs.gojitsu.com/)
- [Getting started](https://docs.gojitsu.com/#/docs/QuickStart.md)
- [API reference](https://docs.gojitsu.com/#/openapi.yaml)
- [Support](https://gojitsu.com/support)
- [Blog](https://gojitsu.com/news-insights)
- [Terms of service](https://gojitsu.com/terms)
- [Privacy policy](https://gojitsu.com/privacy)
- [Status](https://status.gojitsu.com/)

## API Evangelist rating

**Kin Score 49.2** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 60.9 |
| Contract governance | 18.2 |
| Operational transparency | 55.3 |
| Developer ergonomics | 35.1 |
| Access clarity | 43.4 |

**Agent readiness 33.1** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/axlehire/](https://apis.io/providers/axlehire/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for AxleHire (Jitsu): the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/axlehire/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
