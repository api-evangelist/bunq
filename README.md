# bunq (bunq)

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

bunq is a European (Dutch) neobank offering personal and business accounts across the EU. Its Public API is a REST API over HTTPS (`https://api.bunq.com/v1`, sandbox `https://public-api.sandbox.bunq.com/v1`) that lets account holders and licensed third parties read accounts, initiate SEPA and internal payments, send and answer payment requests, manage cards, export statements, upload attachments, and subscribe to event callbacks.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/bunq/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/bunq/refs/heads/main/apis.yml)

## Access Model (read this first)

bunq is **not** a simple bearer-token API. Before any business call you must complete a multi-step handshake and then sign every request:

1. **Installation** — locally generate a 2048-bit RSA key pair (PKCS#8). `POST /v1/installation` with your **public** key. bunq returns an installation `Token` and its `server_public_key`.
2. **Device server** — `POST /v1/device-server` with your bunq **API key** and permitted IPs. Send the installation `Token` in `X-Bunq-Client-Authentication` and sign the request body with your **private** key in `X-Bunq-Client-Signature`.
3. **Session server** — `POST /v1/session-server` to open a session. bunq returns a session `Token` and your user object(s).
4. **Signed calls** — every subsequent request carries the session `Token` in `X-Bunq-Client-Authentication`, and requests are signed with your private key (RSA, PKCS#1 v1.5, SHA-256) in `X-Bunq-Client-Signature`. bunq signs its responses in `X-Bunq-Server-Signature`, which you verify with the `server_public_key`.

Environments: **production** `https://api.bunq.com/v1`, **sandbox** `https://public-api.sandbox.bunq.com/v1`. Every response is wrapped in a top-level `Response` array envelope. Real-time events are delivered by **HTTP webhook callbacks** (`notification-filter-url`, sent from bunq IP range `185.40.108.0/22`) and **push notifications** — bunq does **not** publish a public WebSocket (`wss://`) API.

## Real vs. Modeled

The endpoint paths, handshake, RSA signing scheme, base URLs, rate limits, and callback model are **grounded** in the public bunq documentation at [doc.bunq.com](https://doc.bunq.com). The OpenAPI in this repo is a **representative subset** of a very large API (bunq exposes hundreds of endpoints); request/response **schemas are simplified**, and the generic `Response` envelope and several input bodies are **modeled** rather than copied field-for-field. See `review.yml` for confirmed-vs-modeled details.

## Tags

- Banking
- Neobank
- Payments
- Accounts
- SEPA
- Open Banking
- Fintech
- Europe
- Netherlands

## Timestamps

- **Created:** 2023-11-13
- **Modified:** 2026-07-12

## APIs

### bunq Session (Handshake) API

The three-step API context handshake - POST /installation (register your 2048-bit RSA public key), POST /device-server (register your API key and permitted IPs), and POST /session-server (open a signed session). Returns the session token used for all subsequent calls.

- **Human URL:** [https://doc.bunq.com/basics/authentication](https://doc.bunq.com/basics/authentication)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Authentication
- Installation
- Session

#### Properties

- [Documentation](https://doc.bunq.com/basics/authentication)
- [API Reference](https://doc.bunq.com/basics/signing)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq User API

List and retrieve the user object(s) for the session - UserPerson, UserCompany, or UserPaymentServiceProvider - the top-level identity that owns monetary accounts, cards, and callbacks.

- **Human URL:** [https://doc.bunq.com/basics/bunq-api-objects/user](https://doc.bunq.com/basics/bunq-api-objects/user)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Users
- Accounts
- Identity

#### Properties

- [Documentation](https://doc.bunq.com/basics/bunq-api-objects/user)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Monetary Account API

List and retrieve monetary accounts (MonetaryAccountBank, savings, and joint) with their balance, IBAN alias, currency, limits, and status, and open new bank accounts.

- **Human URL:** [https://doc.bunq.com/basics/bunq-api-objects/monetary-account](https://doc.bunq.com/basics/bunq-api-objects/monetary-account)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Accounts
- Bank
- Savings
- Balance

#### Properties

- [Documentation](https://doc.bunq.com/basics/bunq-api-objects/monetary-account)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Payment API

Execute payments from a monetary account to an IBAN, email, or phone alias (SEPA and internal transfers), list and retrieve executed payments, and create draft payments that require approval before the money moves.

- **Human URL:** [https://doc.bunq.com/payment/payment](https://doc.bunq.com/payment/payment)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Payments
- SEPA
- Transfers
- Draft Payments

#### Properties

- [Documentation](https://doc.bunq.com/payment/payment)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Request Inquiry API

Create and list payment requests (RequestInquiry) sent to a counterparty, and read incoming requests (RequestResponse) addressed to your monetary account so they can be accepted or rejected.

- **Human URL:** [https://doc.bunq.com/request/request-inquiry](https://doc.bunq.com/request/request-inquiry)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Payment Requests
- Request Inquiry
- Request Response

#### Properties

- [Documentation](https://doc.bunq.com/request/request-inquiry)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Card API

List and retrieve the debit and credit cards (CardDebit, CardCredit) linked to a user, including status, limits, and the primary monetary account each card draws from.

- **Human URL:** [https://doc.bunq.com/basics/bunq-api-objects/card](https://doc.bunq.com/basics/bunq-api-objects/card)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Cards
- Debit
- Credit

#### Properties

- [Documentation](https://doc.bunq.com/basics/bunq-api-objects/card)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Attachment API

Upload raw attachment bytes (avatars, receipts, images) and download attachment content by UUID. Content is transferred as binary rather than the JSON response envelope.

- **Human URL:** [https://doc.bunq.com/content-and-exports](https://doc.bunq.com/content-and-exports)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Attachments
- Content
- Uploads

#### Properties

- [Documentation](https://doc.bunq.com/content-and-exports)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Customer Statement (Export) API

Generate account statement exports for a date range in CSV, MT940, or PDF format, list generated statements, and download the resulting file content.

- **Human URL:** [https://doc.bunq.com/customer-statements](https://doc.bunq.com/customer-statements)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Statements
- Exports
- CSV
- MT940
- PDF

#### Properties

- [Documentation](https://doc.bunq.com/customer-statements)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### bunq Notification Filter (Callbacks) API

Manage event callbacks by category. URL notification filters (notification-filter-url) POST real-time events to an HTTPS endpoint you own (webhooks, from bunq IP range 185.40.108.0/22); push notification filters deliver events to the user's bunq mobile app. Delivery is HTTP webhook and push - bunq does not expose a public WebSocket.

- **Human URL:** [https://doc.bunq.com/basics/callbacks-webhooks](https://doc.bunq.com/basics/callbacks-webhooks)
- **Base URL:** `https://api.bunq.com/v1`

#### Tags

- Callbacks
- Webhooks
- Notifications
- Push

#### Properties

- [Documentation](https://doc.bunq.com/basics/callbacks-webhooks)
- [API Reference](https://doc.bunq.com/notification-filter)
- [OpenAPI](openapi/bunq-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bunq.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bunq.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Authentication](authentication/bunq-authentication.yml)
- [Vulnerability Disclosure](security/bunq-vulnerability-disclosure.yml)
- [Domain Security](security/bunq-domain-security.yml)
- [GitHub Organization](https://github.com/bunq)
- [LinkedIn](https://www.linkedin.com/company/bunq)
- [Website](https://www.bunq.com/)
- [Documentation](https://doc.bunq.com/)
- [Developer Portal](https://developer.bunq.com/)
- [Plans](plans/bunq-plans-pricing.yml)
- [Rate Limits](rate-limits/bunq-rate-limits.yml)
- [Fin Ops](finops/bunq-finops.yml)
- [Sandbox](https://beta.doc.bunq.com/basics/sandbox)
- [Status Page](https://status.bunq.com/)
- [Pricing](https://www.bunq.com/en-us/documents/pricing-sheet)
- [Terms of Service](https://www.bunq.com/documents/terms-conditions)
- [Blog](https://medium.com/bunq-developers-corner)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
