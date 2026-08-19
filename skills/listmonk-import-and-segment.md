---
name: listmonk-import-and-segment
description: Bulk-import subscribers from CSV into listmonk, then segment them with SQL expressions and apply bulk list or blocklist changes by query.
api: listmonk
generated: '2026-08-13'
method: generated
source: openapi/_original/listmonk-collections-openapi.yml (https://listmonk.app/docs/swagger/collections.yaml)
operations:
  - importSubscribers
  - getImportSubscribers
  - getImportSubscriberLogs
  - stopImportSubscribers
  - getSubscribers
  - manageSubscriberListsByQuery
  - blocklistSubscribersQuery
  - deleteSubscriberByQuery
  - exportSubscriberDataByID
---

# Import and segment subscribers

Two capabilities that only exist together in listmonk: a CSV importer that runs
as a singleton background job, and a segmentation engine that takes **raw
Postgres SQL**.

Base URL `https://{host}/api`. Auth `-u "api_user:token"`.

## Permissions

`subscribers:import` for the import. `subscribers:get_all` to read.
`subscribers:manage` to modify. And for anything using a `query`,
**`subscribers:sql_query`** — which listmonk documents as high risk, because a
read-only SQL expression still supersedes every per-list and per-subscriber grant
the user has, and can reach Postgres system functions. Do not attach it to a
general-purpose API user.

## Import

1. `importSubscribers` — `POST /api/import/subscribers`, `multipart/form-data`:
   the CSV (or a ZIP of one) as `file`, plus a `params` JSON field:
   ```json
   {
     "mode": "subscribe",
     "subscription_status": "unconfirmed",
     "delim": ",",
     "lists": [3],
     "overwrite": true
   }
   ```
   `mode` is `subscribe` or `blocklist`. `subscription_status` is `unconfirmed`
   or `confirmed` — setting `confirmed` on a double opt-in list marks everyone as
   having consented, so be sure you actually have that consent. `overwrite`
   decides whether existing subscriber names and attribs are replaced.

   The CSV needs `email`, `name`, and an `attributes` column holding a JSON
   object per row.

2. **One import at a time per instance.** `/api/import/subscribers` is a
   singleton. Starting a second while one runs will not queue — check first.

3. `getImportSubscribers` — `GET /api/import/subscribers` returns the running
   import's `{name, total, imported, status}`. Poll it. Status ends at
   `finished`, `stopped` or `failed`.

4. `getImportSubscriberLogs` — `GET /api/import/subscribers/logs` tails the
   importer's log. This is where per-row failures appear; the status endpoint
   only gives you counts. Always read the log before declaring an import clean.

5. `stopImportSubscribers` — `DELETE /api/import/subscribers` aborts. Rows
   already written stay written; this is not a rollback.

## Segment

`getSubscribers` — `GET /api/subscribers` with a `query` parameter holding a
Postgres boolean expression over the `subscribers` table:

```
query=subscribers.name LIKE 'John%' AND subscribers.attribs->>'city' = 'Bengaluru'
query=subscribers.attribs->>'plan' = 'pro' AND subscribers.status = 'enabled'
query=subscribers.created_at > now() - interval '30 days'
```

Combine with `list_id` (repeatable) to scope to lists, and the usual `page` /
`per_page` / `order_by` / `order`. `per_page=all` returns everything in one
response — convenient, and a way to pull a very large payload by accident.

Always run the query as a read (`getSubscribers`) and look at `data.total` before
you run the same query as a mutation. There is no dry-run mode and no undo.

## Bulk mutate by query

- `manageSubscriberListsByQuery` — `PUT /api/subscribers/query/lists`. Add,
  remove or mark-unsubscribed across every subscriber the query matches:
  ```json
  {
    "query": "subscribers.email LIKE '%@example.com'",
    "list_ids": [3],
    "target_list_ids": [7],
    "action": "add",
    "status": "confirmed"
  }
  ```
- `blocklistSubscribersQuery` — `PUT /api/subscribers/query/blocklist`.
  Blocklisting is global: it stops all mail to that subscriber across every list,
  and it is not a per-list opt-out.
- `deleteSubscriberByQuery` — `POST /api/subscribers/query/delete`. Permanent.
  Run the query as a read first, every time.

## Data subject requests

`exportSubscriberDataByID` — `GET /api/subscribers/{id}/export` returns one
subscriber's profile, subscriptions, campaign views and link clicks. This is the
operation to reach for on a GDPR access request; pair it with
`deleteSubscriberById` for erasure.

## Cautions

- **No idempotency, no transaction.** A bulk query mutation that fails partway
  leaves partial changes. Re-running is not free — `add` is idempotent in effect,
  `delete` is not.
- SQL is passed through to Postgres. A syntax error comes back as a 400 or 422
  with the database's message in `{"message": "..."}` — useful, and also a
  reminder that this parameter is a genuine query interface, not a filter DSL.
- Errors carry no machine-readable code. Branch on status, log the message.
