---
name: listmonk-bounce-handling
description: Record bounce events into listmonk from your own tooling or an SMTP provider webhook, then read and act on the resulting bounce records.
api: listmonk
generated: '2026-08-13'
method: generated
source: >-
  openapi/_original/listmonk-collections-openapi.yml
  (https://listmonk.app/docs/swagger/collections.yaml) and
  https://listmonk.app/docs/bounces/
operations:
  - getBounces
  - getBounceById
  - deleteBounces
  - deleteBounceById
  - getSubscriberBouncesById
  - deleteSubscriberBouncesById
  - blocklistSubscribersQuery
  - manageBlocklistSubscribersById
---

# Handle bounces in listmonk

listmonk's event surface points **inward**. It does not emit webhooks; it
receives them. Bounce records arrive one of three ways — POP3 mailbox scanning,
your own script posting to the generic webhook, or a provider-specific receiver —
and then become records you can query and act on.

Base URL `https://{host}/api`. Auth `-u "api_user:token"`.

## Before anything works

Bounce processing must be enabled in **Settings -> Bounces**. Until it is, the
webhook endpoints and the bounce APIs are not available at all. This is the first
thing to check when a bounce integration appears to do nothing.

Permissions: `bounces:get` to read, `bounces:manage` to delete, and
**`webhooks:post_bounce`** for the ingest endpoint.

## Ingest — your own tooling

`POST /webhooks/bounce` (note: `/webhooks/...`, **not** under `/api`):

```shell
curl -u 'api_username:access_token' -X POST 'https://your-host/webhooks/bounce' \
  -H "Content-Type: application/json" \
  --data '{
    "email": "user1@mail.com",
    "campaign_uuid": "9f86b50d-5711-41c8-ab03-bc91c43d711b",
    "source": "api",
    "type": "hard",
    "meta": "{\"additional\": \"info\"}"
  }'
```

- `source` and `type` are required. Everything else is optional, except that you
  must supply **either** `email` **or** `subscriber_uuid`.
- `campaign_uuid` is a **UUID**, not the integer campaign id. Mixing the two up
  is the usual failure here: the public and webhook surfaces are uuid-addressed
  while the admin API is integer-addressed.
- `meta` is an **escaped JSON string**, not a nested object.
- `type` is `hard` or `soft`. listmonk's docs are explicit that this currently
  has no effect on how the bounce is treated — do not build logic expecting
  listmonk to act differently on hard versus soft.

## Ingest — SMTP provider webhooks

Point the provider at the matching receiver on your instance and configure the
shared secret or signature under Settings -> Bounces:

| Provider | Endpoint |
|---|---|
| Amazon SES | `/webhooks/service/ses` |
| Azure Communication Services | `/webhooks/service/azure` |
| Sendgrid / Twilio | `/webhooks/service/sendgrid` |
| Postmark | `/webhooks/service/postmark` |
| Forward Email | `/webhooks/service/forwardemail` |
| Lettermint | `/webhooks/service/lettermint` |

Each accepts that provider's native payload. If you are on SES, listmonk
recommends this over POP3 scanning for sender-reputation reasons.

## Read

- `getBounces` — `GET /api/bounces`. Filter with `campaign_id` and `source`;
  paginate with `page` / `per_page`; sort with `order_by`
  (`email`, `campaign_name`, `source`, `created_at`) and `order` (`asc`/`desc`).
- `getBounceById` — `GET /api/bounces/{id}`.
- `getSubscriberBouncesById` — `GET /api/subscribers/{id}/bounces` for one
  subscriber's history. This is the one to call before deciding whether a single
  address is really dead.

## Act

listmonk records bounces; deciding what they mean is yours.

- Single address: `manageBlocklistSubscribersById` —
  `PUT /api/subscribers/{id}/blocklist`.
- In bulk: `blocklistSubscribersQuery` — `PUT /api/subscribers/query/blocklist`
  with a SQL expression. Requires `subscribers:sql_query`. Read the query with
  `getSubscribers` first and check `data.total` — blocklisting is global across
  every list, not a per-list opt-out.
- Housekeeping: `deleteBounceById`, `deleteBounces` (all or by id list), and
  `deleteSubscriberBouncesById` clear records. Deleting the record does not
  un-blocklist anyone.

## Cautions

- The webhook path is `/webhooks/bounce`, outside `/api`. Do not prefix it.
- No idempotency: posting the same bounce event twice creates two records.
  Deduplicate upstream if your source can replay.
- Errors are `{"message": "..."}` with no code. A 404 on the webhook usually
  means bounce processing is switched off, not that the URL is wrong.

See `../asyncapi/listmonk-bounce-webhooks-asyncapi.yml` for the full channel and
payload model.
