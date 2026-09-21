---
name: check-minecraft-status
description: >-
  Read the cached status of the core Minecraft online services (Microsoft/Mojang
  sign-in, session, Realms) from NewMinecraftServers to tell a player whether an
  outage is on their side or Minecraft's.
api: NewMinecraftServers API
operations:
- getMinecraftServiceStatus
---

# Check Minecraft service status

A single read-only GET under `https://newminecraftservers.net/api/v1`, no key
required.

## Steps

1. Call `getMinecraftServiceStatus`.
2. Read `data.overall` for the rolled-up state and iterate `data.checks[]` for each
   monitored service (id, label, host, current check state). `checkedAt` and
   `intervalSeconds` tell you how fresh the reading is (checked ~every 60s).
3. Report the failing service(s) to the user, or confirm all services are
   operational.

## Conventions

- Responses are cached (`Cache-Control: public, max-age=30`); do not poll faster
  than the check interval.
- On `429`, honor the `Retry-After` header before retrying.
