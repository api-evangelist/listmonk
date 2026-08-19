---
name: listmonk-campaign-send
description: Create, preview, test and send a listmonk campaign to one or more lists, then read its delivery stats and analytics.
api: listmonk
generated: '2026-08-13'
method: generated
source: openapi/_original/listmonk-collections-openapi.yml (https://listmonk.app/docs/swagger/collections.yaml)
operations:
  - getTemplates
  - getLists
  - createCampaign
  - updateCampaignById
  - createCampaignContentById
  - previewCampaignById
  - updatePreviewCampaignById
  - testCampaignById
  - updateCampaignStatusById
  - getRunningCampaignStats
  - getCampaignById
  - getCampaignAnalytics
  - updateCampaignArchiveById
---

# Send a campaign with listmonk

The one thing to internalise: **sending is a status transition, not a send
endpoint.** There is no `POST /campaigns/{id}/send`. You move the campaign's
status to `running` and listmonk's workers pick it up. Everything else follows
from that.

Base URL `https://{host}/api`. Auth `-u "api_user:token"`.

## Permissions

`campaigns:manage` (or `campaigns:manage_all`), `lists:get_all` or a List role
covering the target lists, `templates:get`, and — separately —
**`campaigns:send`**. Since v6.1.0 that last one is split out: an API user with
`campaigns:manage_all` still cannot send. If your status transition returns a
permission error and the campaign is otherwise fine, this is why.

## Steps

1. `getLists` — `GET /api/lists`. Collect the integer ids of the target lists.
   Check `subscriber_count` so you know the size of what you are about to send.
2. `getTemplates` — `GET /api/templates`. Pick a template whose `type` is
   `campaign` or `campaign_visual` (`tx` templates are for transactional mail and
   will not work here). Note its `id`, or rely on the default.
3. `createCampaign` — `POST /api/campaigns`:
   ```json
   {
     "name": "August newsletter",
     "subject": "What shipped in August",
     "lists": [3, 7],
     "from_email": "News <news@example.com>",
     "content_type": "richtext",
     "messenger": "email",
     "type": "regular",
     "tags": ["newsletter"],
     "template_id": 2,
     "send_later": false
   }
   ```
   The campaign is created as `draft`. `name` is internal; `subject` is what
   recipients see.
4. Set the body — either `updateCampaignById` (`PUT /api/campaigns/{id}` with
   `body` / `altbody`) or `createCampaignContentById`
   (`POST /api/campaigns/{id}/content`). Bodies are Go templates with Sprig
   functions available, so `{{ .Subscriber.FirstName }}` and friends resolve per
   recipient.
5. **Preview before you send.** `previewCampaignById`
   (`GET /api/campaigns/{id}/preview`) returns rendered `text/html` — not JSON.
   `previewCampaignTextById` (`POST /api/campaigns/{id}/text`) gives the plain
   text part. `updatePreviewCampaignById` previews an unsaved body.
6. **Test send.** `testCampaignById` — `POST /api/campaigns/{id}/test` with a
   list of e-mail addresses. This is the only way to see the real thing through a
   real mail client before it goes out. Do this every time.
7. **Send.** `updateCampaignStatusById` — `PUT /api/campaigns/{id}/status` with
   `{"status": "running"}`.

   The valid transitions are `draft -> scheduled|running`,
   `running -> paused|cancelled`, `paused -> running|cancelled`,
   `running -> finished` (listmonk sets this itself). You cannot un-finish a
   campaign, and you cannot edit one that is running.

## Scheduling instead of sending now

Set `send_later: true` and `send_at` (RFC 3339 with offset, e.g.
`2026-09-01T09:00:00+05:30`) on the campaign, then transition the status to
`scheduled`. listmonk's scheduler starts it at that time.

## While it runs

- `getRunningCampaignStats` — `GET /api/campaigns/running/stats` for the live
  picture across all running campaigns: `to_send`, `sent`, `rate`, `net_rate`.
- `getCampaignById` — `GET /api/campaigns/{id}` for one campaign's counters.
- To stop: `PUT /api/campaigns/{id}/status` with `paused`, or `cancelled` to end
  it. **Cancelling does not recall mail already sent.**

## Afterwards

- `getCampaignAnalytics` — `GET /api/campaigns/analytics/{type}` where type is
  `views`, `clicks`, `bounces` or `links`, over a date range. Requires
  `campaigns:get_analytics`. Returns nothing useful if the operator has disabled
  tracking under the global Privacy setting (v6.1.0+).
- `updateCampaignArchiveById` — `PUT /api/campaigns/{id}/archive` publishes the
  campaign as a public web page. Be deliberate: an archived campaign is world
  readable, and listmonk renders whatever HTML the body contains, scripts
  included. That is documented and intended behaviour, not a bug.

## Cautions

- **No idempotency, and sending is irreversible.** If a `PUT .../status` call
  times out, do **not** blindly retry. Read the campaign back with
  `getCampaignById` and look at `status`, `sent` and `started_at` first.
- Errors are `{"message": "..."}` with no error code. 403 means the credential or
  the permission is wrong; the message is the only detail you get.
- `previewCampaignById` returns HTML, not the `{"data": ...}` envelope. Do not
  hand it to a JSON parser.
