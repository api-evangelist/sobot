---
name: Provision and manage Sobot agents
description: List, create, edit, and role-manage Sobot customer-service agents and departments.
api: openapi/sobot-basic-openapi.json
base_url: https://sg.sobot.io
operations:
- queryAgentInfoList
- queryDepartAgentInfoList
- saveAgentInfo
- updateAgentInfo
- batchUpdateAgentRole
---

# Provision and manage Sobot agents

Requests go to `https://sg.sobot.io` with an HTTP Bearer access token. Bodies are
JSON; status is returned in the `code` / `msg` envelope with results under `data`.

## Steps

1. **List agents** — `GET /agent/queryAgentInfoList` to review existing seats,
   or `POST /agent/queryDepartAgentInfoList` to scope by department.
2. **Create an agent** — `POST /agent/saveAgentInfo` with the new seat's profile.
3. **Edit an agent** — `POST /agent/updateAgentInfo` to change an existing seat.
4. **Adjust roles in bulk** — `POST /agent/batchUpdateAgentRole` to reassign
   roles across multiple agents at once.

## Rules

- Confirm the target department exists before assigning agents to it.
- Check `code` on every response; treat a non-success `code` as a failed write.
- Prefer `batchUpdateAgentRole` over per-agent role edits for large changes.
