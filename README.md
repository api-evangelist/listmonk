# listmonk (listmonk)

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

listmonk is a free and open-source, self-hosted newsletter and mailing-list manager built in Go with a Vue front end. Every feature in the admin UI is backed by a documented REST API on the self-hosted instance (Basic auth with an API user and token) covering subscribers, lists, campaigns, templates, media, CSV import, transactional messages, and bounces. There is no hosted SaaS - users run their own instance.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/listmonk/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/listmonk/refs/heads/main/apis.yml)

## Tags

- Email
- Newsletter
- Mailing List
- Open Source
- Self-Hosted

## Timestamps

- **Created:** 2026-06-25
- **Modified:** 2026-06-25

## APIs

### listmonk Subscribers API

Create, query, update, blocklist, and delete subscribers, manage their list memberships individually or in bulk via SQL query expressions, and export subscriber data. Self-hosted endpoints secured with Basic auth (api_user:token).

- **Human URL:** [https://listmonk.app/docs/apis/subscribers/](https://listmonk.app/docs/apis/subscribers/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Subscribers
- Contacts
- Lists

#### Properties

- [Documentation](https://listmonk.app/docs/apis/subscribers/)
- [API Reference](https://listmonk.app/docs/apis/apis/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### listmonk Lists API

Create, retrieve, update, and delete mailing lists (single and double opt-in, public and private), with a public unauthenticated endpoint for surfacing subscribable lists on subscription forms.

- **Human URL:** [https://listmonk.app/docs/apis/lists/](https://listmonk.app/docs/apis/lists/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Lists
- Mailing Lists
- Segmentation

#### Properties

- [Documentation](https://listmonk.app/docs/apis/lists/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### listmonk Campaigns API

Create, schedule, test, preview, and send email campaigns to one or more lists, change campaign status, publish to a public archive, and pull running stats and view/click/bounce analytics.

- **Human URL:** [https://listmonk.app/docs/apis/campaigns/](https://listmonk.app/docs/apis/campaigns/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Campaigns
- Email
- Analytics

#### Properties

- [Documentation](https://listmonk.app/docs/apis/campaigns/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### listmonk Templates API

Manage campaign and transactional message templates - create, retrieve, update, delete, set a default, and render HTML previews of Go-templated email bodies.

- **Human URL:** [https://listmonk.app/docs/apis/templates/](https://listmonk.app/docs/apis/templates/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Templates
- HTML
- Rendering

#### Properties

- [Documentation](https://listmonk.app/docs/apis/templates/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### listmonk Media API

Upload, list, retrieve, and delete media files (images and attachments) stored in the local filesystem or an S3-compatible backend for use in campaigns and templates.

- **Human URL:** [https://listmonk.app/docs/apis/media/](https://listmonk.app/docs/apis/media/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Media
- Uploads
- Files

#### Properties

- [Documentation](https://listmonk.app/docs/apis/media/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### listmonk Transactional API

Send arbitrary transactional messages (welcome emails, order confirmations, password resets) to one or more subscribers through a preconfigured transactional template, with custom data, headers, attachments, and messenger selection.

- **Human URL:** [https://listmonk.app/docs/apis/transactional/](https://listmonk.app/docs/apis/transactional/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Transactional
- Messaging
- Email

#### Properties

- [Documentation](https://listmonk.app/docs/apis/transactional/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### listmonk Import and Bounces API

Bulk import subscribers from a CSV (optionally ZIP-compressed) file and monitor or stop the running import, plus retrieve and delete bounce records used to maintain sender reputation.

- **Human URL:** [https://listmonk.app/docs/apis/import/](https://listmonk.app/docs/apis/import/)
- **Base URL:** `http://localhost:9000/api`

#### Tags

- Import
- Bounces
- Deliverability

#### Properties

- [Documentation](https://listmonk.app/docs/apis/import/)
- [Documentation](https://listmonk.app/docs/apis/bounces/)
- [OpenAPI](openapi/listmonk-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/listmonk.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/listmonk.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [GitHub Organization](https://github.com/knadh/listmonk)
- [LinkedIn](https://www.linkedin.com/in/knadh)
- [Website](https://listmonk.app)
- [Documentation](https://listmonk.app/docs/apis/apis/)
- [Plans](plans/listmonk-plans-pricing.yml)
- [Rate Limits](rate-limits/listmonk-rate-limits.yml)
- [Fin Ops](finops/listmonk-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
