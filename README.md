# listmonk (listmonk)

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
