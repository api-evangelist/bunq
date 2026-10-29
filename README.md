# bunq

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

bunq is a European (Dutch) neobank offering personal and business accounts across the EU. Its Public API is a REST API over HTTPS (https://api.bunq.com/v1, sandbox https://public-api.sandbox.bunq.com/v1) that lets account holders and licensed third parties read accounts, initiate SEPA and internal payments, send and answer payment requests, manage cards, export statements, upload attachments, and subscribe to event callbacks. bunq uses a distinctive multi-step handshake - installation (register an RSA public key), device-server, then session-server - after which requests are RSA-signed with X-Bunq-Client-Signature and authenticated with a session token in X-Bunq-Client-Authentication.

**Areas:** Banking · Neobank · Payments · Account · SEPA · Open Banking · Fintech · Europe · Netherlands

## bunq on APIs.io

Everything profiled for bunq is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/bunq/](https://apis.io/providers/bunq/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [bunq Attachment API](https://apis.io/apis/bunq/bunq-attachment-api/) — Upload and retrieve attachment content.
- [bunq Card API](https://apis.io/apis/bunq/bunq-card-api/) — Debit and credit cards linked to monetary accounts.
- [bunq Customer Statement API](https://apis.io/apis/bunq/bunq-customer-statement-api/) — Generate and download account statement exports (CSV, MT940, PDF).
- [bunq Handshake API](https://apis.io/apis/bunq/bunq-handshake-api/) — Installation, device registration, and session bootstrap. Establishes the RSA-signed API context.
- [bunq Monetary Account API](https://apis.io/apis/bunq/bunq-monetary-account-api/) — Bank, savings, and joint accounts holding money.
- [bunq Notification Filter API](https://apis.io/apis/bunq/bunq-notification-filter-api/) — Manage URL (webhook) and push notification callbacks for account events.
- [bunq Payment API](https://apis.io/apis/bunq/bunq-payment-api/) — Execute and read payments (including SEPA) for a monetary account.
- [bunq Request Inquiry API](https://apis.io/apis/bunq/bunq-request-inquiry-api/) — Create and read payment requests (money you ask another party to pay).
- [bunq User API](https://apis.io/apis/bunq/bunq-user-api/) — The authenticated user (person, company, or payment service provider).

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Bunq Agentic Access](https://apis.io/agentic-access/bunq/bunq-agentic-access/)

#### Collections (10)

Postman and Open Collection request collections.

- [bunq Public API](https://apis.io/collections/bunq/open-bunq/)
- [bunq Public Attachment API](https://apis.io/collections/bunq/open-bunq-attachment-api/)
- [bunq Public Attachment Card API](https://apis.io/collections/bunq/open-bunq-card-api/)
- [bunq Public Attachment Customer Statement API](https://apis.io/collections/bunq/open-bunq-customer-statement-api/)
- [bunq Public Attachment Handshake API](https://apis.io/collections/bunq/open-bunq-handshake-api/)
- [bunq Public Attachment Monetary Account API](https://apis.io/collections/bunq/open-bunq-monetary-account-api/)
- [bunq Public Attachment Notification Filter API](https://apis.io/collections/bunq/open-bunq-notification-filter-api/)
- [bunq Public Attachment Payment API](https://apis.io/collections/bunq/open-bunq-payment-api/)
- [bunq Public Attachment Request Inquiry API](https://apis.io/collections/bunq/open-bunq-request-inquiry-api/)
- [bunq Public Attachment User API](https://apis.io/collections/bunq/open-bunq-user-api/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Bunq Authentication](https://apis.io/security/bunq/bunq-authentication/)
- [Bunq Domain Security](https://apis.io/security/bunq/bunq-domain-security/)
- [Bunq Vulnerability Disclosure](https://apis.io/security/bunq/bunq-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Bunq Plans Pricing](https://apis.io/plans/bunq/bunq-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Bunq Rate Limits](https://apis.io/rate-limits/bunq/bunq-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Bunq Finops](https://apis.io/finops/bunq/bunq-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.bunq.com/)
- [Documentation](https://doc.bunq.com/)
- [Pricing](https://www.bunq.com/en-us/documents/pricing-sheet)
- [Blog](https://medium.com/bunq-developers-corner)
- [Terms of service](https://www.bunq.com/documents/terms-conditions)
- [GitHub](https://github.com/bunq)
- [Status](https://status.bunq.com/)

## API Evangelist rating

**Kin Score 34.5** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 50.8 |
| Contract governance | 0.0 |
| Operational transparency | 26.3 |
| Developer ergonomics | 35.7 |
| Access clarity | 28.9 |

**Agent readiness 17.9** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | — |
| OpenAPI Examples | — |
| Rate Limit Signal | derived (partial credit) |
| Event Surface Described | — |
| Agent Skills | — |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 17.3** — Banking & Open Finance

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/bunq/](https://apis.io/providers/bunq/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for bunq: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/bunq/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
