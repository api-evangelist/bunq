# bunq (bunq)

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
