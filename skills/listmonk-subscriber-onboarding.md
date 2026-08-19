---
name: listmonk-subscriber-onboarding
description: Add a subscriber to a listmonk list and drive the double opt-in confirmation, including the public unauthenticated path a signup form would use.
api: listmonk
generated: '2026-08-13'
method: generated
source: openapi/_original/listmonk-collections-openapi.yml (https://listmonk.app/docs/swagger/collections.yaml)
operations:
  - getLists
  - createList
  - getPublicLists
  - handlePublicSubscription
  - createSubscriber
  - getSubscriberById
  - subscriberSendOptinById
  - manageSubscriberListById
---

# Onboard a subscriber to a listmonk list

listmonk has two ways in, and picking the wrong one is the most common mistake.
The **public** path is unauthenticated and is what a signup form posts to. The
**admin** path requires an API user and lets you set status directly, including
skipping confirmation. Decide which you are before you start.

Base URL is the operator's own instance: `https://{host}/api`. There is no shared
listmonk host.

## Prerequisites

- An API user with `lists:get_all` (or a List role covering the target list) and
  `subscribers:manage`. Created under Admin -> Users.
- Auth on every admin call: `-u "api_user:token"` or
  `-H "Authorization: token api_user:token"`.

## Path A — public signup form (no credentials)

1. `getPublicLists` — `GET /api/public/lists`. Returns only lists marked public,
   as `{uuid, name}`. Use the **uuid**, not the integer id: the public surface is
   uuid-addressed.
2. `handlePublicSubscription` — `POST /api/public/subscription` with the
   subscriber's email, name and the list uuid(s).

This requires `app.enable_public_subscription_page` to be on. If the operator
disabled it, the endpoint is refused — since v5.0.3 listmonk explicitly rejects
the call rather than silently accepting it.

## Path B — admin API

1. `getLists` — `GET /api/lists` to resolve the target list's integer `id`, its
   `type` (`public|private`) and its `optin` mode (`single|double`). The opt-in
   mode decides everything that follows.
2. If the list does not exist, `createList` — `POST /api/lists` with
   `{name, type, optin, tags, description}`.
3. `createSubscriber` — `POST /api/subscribers`:
   ```json
   {
     "email": "user@example.com",
     "name": "User",
     "status": "enabled",
     "lists": [3],
     "preconfirm_subscriptions": false,
     "attribs": {"city": "Bengaluru", "plan": "pro"}
   }
   ```
   Two fields carry the whole decision:
   - `status` is the **subscriber's** status (`enabled|disabled|blocklisted`).
   - `preconfirm_subscriptions` controls the **subscription's** status. Leave it
     `false` on a double opt-in list to create the subscription as
     `unconfirmed`; set it `true` only when you have a lawful basis to skip
     confirmation, because it marks the subscription `confirmed` without the
     subscriber ever agreeing.

   These are different things on different entities. A subscriber can be
   `enabled` while every one of their subscriptions is `unconfirmed`.
4. `subscriberSendOptinById` — `POST /api/subscribers/{id}/optin` to send (or
   resend) the confirmation e-mail. Only meaningful for double opt-in lists.
5. `getSubscriberById` — `GET /api/subscribers/{id}` to read back
   `lists[].subscription_status` and confirm it moved
   `unconfirmed -> confirmed`. Confirmation is asynchronous; it happens when the
   human clicks, so poll or move on — do not block.

## Adding an existing subscriber to another list

`manageSubscriberListById` — `PUT /api/subscribers/lists/{id}` with the target
list ids and the action. Use this rather than re-POSTing the subscriber: a repeat
`createSubscriber` on an existing e-mail is a conflict, not an upsert.

## Rules that apply to every call here

- **No idempotency.** There is no Idempotency-Key. A retried `createSubscriber`
  is a second attempt at creating the same e-mail, not a no-op. Check with
  `getSubscribers?query=subscribers.email='...'` before retrying a call whose
  response you did not see.
- **403, not 401**, on a bad or missing credential, with body
  `{"message":"invalid session"}` and no WWW-Authenticate header.
- Errors are `{"message": "..."}`. There is no error code to switch on — branch
  on the HTTP status and log the message.
- Success is `{"data": {...}}`.
- `attribs` is free-form JSONB and is what later segmentation queries read.
  Write it at creation time; retrofitting it across a list is a bulk update.

See `../conventions/listmonk-conventions.yml` and
`../errors/listmonk-problem-types.yml`.
