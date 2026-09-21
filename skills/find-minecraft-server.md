---
name: find-minecraft-server
description: >-
  Discover a published Minecraft server on NewMinecraftServers by search terms,
  gamemode, edition or online state, resolve one by its IP/address, and read its
  full listing and recent player-count history.
api: NewMinecraftServers API
operations:
- listServers
- lookupServerByAddress
- getServerBySlug
- getPublicServerHistory
---

# Find a Minecraft server

All requests are read-only GETs under `https://newminecraftservers.net/api/v1`.
No API key is required and CORS is enabled. Successful responses wrap the payload
in `data`; failures return `{ error: { code, message } }`.

## Steps

1. **Search the directory** — call `listServers` with `q` (1-160 chars),
   `family`, `mode`, `edition`, `outcome` and `sort` (newest, ranked, relevance).
   Page with `page` and `limit` (1-25); the response carries `page`, `total` and
   `totalPages`, so loop until `page` reaches `totalPages`.
2. **Resolve a known address** — if you already have an IP/host, call
   `lookupServerByAddress` with the `address` query parameter. A `404` means no
   published listing matched.
3. **Open one listing** — call `getServerBySlug` with the server `slug` from the
   search or lookup result for the full detail envelope (connection, latest check,
   gamemodes, MOTD).
4. **Read history** — call `getPublicServerHistory` with the `slug` and an
   optional `range` for cached player-count snapshots.

## Conventions

- Dates are ISO 8601 UTC strings.
- A `429` with a `Retry-After` header (about 60s) signals the per-process request
  limit; back off and retry after the header value.
