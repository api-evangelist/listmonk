---
name: listmonk-transactional-send
description: Send a one-off transactional message through listmonk using a tx template, including the retry and deduplication rules the API does not give you.
api: listmonk
generated: '2026-08-13'
method: generated
source: openapi/_original/listmonk-collections-openapi.yml (https://listmonk.app/docs/swagger/collections.yaml)
operations:
  - getTemplates
  - createTemplate
  - previewTemplate
  - transactWithSubscriber
---

# Send a transactional message

One endpoint: `POST /api/tx`. It renders a `tx`-type template with the data you
pass and sends it immediately — no campaign, no list, no queue you can inspect.

Base URL `https://{host}/api`. Auth `-u "api_user:token"`. Permission: **`tx:send`**.

## Steps

1. `getTemplates` — `GET /api/templates`. You need a template whose `type` is
   `tx`. Campaign templates will not work here.
2. If you need one, `createTemplate` — `POST /api/templates` with
   `{"name": "...", "type": "tx", "subject": "...", "body": "..."}`. The subject
   and body are Go templates; your payload is available under `{{ .Tx.Data }}`.
3. `previewTemplate` — `POST /api/templates/preview` renders a template body
   without saving it. Returns `text/html`. Use it to check your placeholders
   resolve before you send anything to a real address.
4. `transactWithSubscriber` — `POST /api/tx`:
   ```json
   {
     "subscriber_email": "user@example.com",
     "template_id": 5,
     "from_email": "Support <support@example.com>",
     "data": {"order_id": "A-1029", "total": "42.00"},
     "headers": [{"X-Order-Id": "A-1029"}],
     "messenger": "email",
     "content_type": "html"
   }
   ```

## Targeting

Identify the recipient with **either** `subscriber_email` **or** `subscriber_id`,
not both. Since v6.0.0 the address does not have to belong to an existing
subscriber — you can send to a non-subscriber. On earlier versions the recipient
had to exist first.

Attachments: `POST /api/tx` also accepts `multipart/form-data`, where the JSON
body goes in a `data` field and files are attached alongside.

## The retry problem — read this

There is **no idempotency key** on this endpoint, and no de-duplication window.
listmonk will happily send the same order confirmation twice. The API gives you
nothing to prevent that, so the responsibility is yours:

- Generate your own idempotency key per logical message and store it before you
  call.
- On a timeout or a 5xx, **do not auto-retry blind**. There is no way to ask
  listmonk "did you already send this one" — transactional messages are not
  stored as a queryable resource. A retry is a real second e-mail.
- If you must retry, cap it at one, and only for connection-level failures where
  you have evidence the request never reached the server.
- Carry your key in `headers` (e.g. `X-Idempotency-Key`) so it lands in the
  message headers and shows up in your SMTP provider's logs. That is the only
  audit trail you will get.

## Other cautions

- **Rate limiting is the operator's and the SMTP provider's**, not listmonk's.
  listmonk documents 429 but returns no `RateLimit-*` or `Retry-After` header.
  Sending concurrency and per-messenger message rates are configured under
  Settings -> Performance; downstream, SES or Sendgrid enforces its own quota.
  Sustained bulk transactional traffic will hit one of those two ceilings, and
  the signal you get back is a failure, not a header.
- Errors are `{"message": "..."}` with no machine-readable code. 403 with
  `{"message":"invalid session"}` means the credential is wrong; a permission
  failure also surfaces as prose.
- `messenger` defaults to `email`. Other messengers must be configured by the
  operator in Settings first.

See `../conventions/listmonk-conventions.yml` and
`../errors/listmonk-problem-types.yml`.
