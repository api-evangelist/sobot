---
name: Handle a Sobot online chat conversation
description: Connect a visitor, exchange messages with bot and human agents, and close a Sobot live-chat session.
api: openapi/sobot-online-openapi.json
base_url: https://sg.sobot.io
operations:
- chat_connect
- chat_send
- robot_chat
- robot_feedback
- out
- get_detail_by_cid
---

# Handle a Sobot online chat conversation

All requests go to `https://sg.sobot.io` with an HTTP Bearer access token in the
`Authorization` header (see `authentication/sobot-authentication.yml`). Bodies
are JSON. Status is returned in the `retCode` / `retMsg` envelope fields.

## Steps

1. **Connect the visitor** — `POST /api/5/user/chat_connect`. Supply a unique
   `partnerid` (your stable visitor key) and `source` (channel code, e.g. 0
   desktop web, 23 WhatsApp). The response identifies the conversation (`cid`).
2. **Send a message** — `POST /api/5/user/chat_send` with the visitor's text.
3. **Optionally run the bot** — `POST /api/5/user/robot_chat` for an AI answer,
   and `POST /api/5/user/robot_feedback` to record whether the answer helped.
4. **Read session detail** — `POST /api/5/user/get_detail_by_cid` using the
   `cid` to pull the conversation record.
5. **End the session** — `POST /api/5/user/out` when the visitor leaves.

## Rules

- Reuse the same `partnerid` for a returning visitor so sessions link to one user.
- Check `retCode` on every response before proceeding; surface `retMsg` on failure.
- No idempotency-key header exists — do not blindly retry writes; re-query state instead.
