---
name: Manage Sobot voice extensions
description: Provision, search, and monitor Sobot voice / call-center softphone extensions.
api: openapi/sobot-voice-openapi.json
base_url: https://sg.sobot.io
operations:
- search_extensions
- add_extension
- delete_extension
- set_extension_password
- extension_registration_status
---

# Manage Sobot voice extensions

Requests go to `https://sg.sobot.io` with an HTTP Bearer access token. Bodies are
JSON; status is returned in the `code` / `msg` envelope.

## Steps

1. **Find capacity** — `GET /exts/count` for the company's extension count and
   `POST /exts/idle/search` (or `/exts/idle/page/search`) for unused extensions.
2. **Create an extension** — `POST /api/exts/_add` (the open-API create endpoint).
   Note `POST /exts` is deprecated — do not use it.
3. **Set the password** — `PUT /exts/{id}` to set/rotate the extension password.
4. **Search / detail** — `POST /exts/search` (paged) and `POST /exts/detail`.
5. **Monitor registration** — on a `webmsg` `EventUnreachable` or
   `EventUnregistered` event, call
   `GET /exts/registered/{ext}/{phoneType}/{ip}` to read the current state.
6. **Delete** — `DELETE /exts/{id}` to remove an extension.

## Rules

- Bind an extension to an agent (`agentUUID`) before it can take calls.
- React to `webmsg` softphone events rather than polling registration state.
- Check `code` on every response before proceeding.
