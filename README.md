# pelican-eggs

Custom [Pelican](https://pelican.dev) eggs for the game servers we host.
One directory per game; each egg ships in two formats:

- `*.yaml` — Pelican-native (`PLCN_v3`), the primary format
- `*.json` — Pterodactyl-compatible (`PTDL_v2`) fallback

Import via *Admin → Eggs → Import* (re-import onto the existing egg to update;
servers keep their data).

## palworld

`palworld/egg-palworld-official-docker.yaml` — Palworld dedicated server
installed from the **official Pocketpair Docker image** (layers pulled straight
from GHCR and extracted; world data preserved). Updates run automatically at
startup while the "Auto Update" variable is on (digest check against the
official image); panel "Reinstall" always force-refreshes.

Highlights:

- Runtime image: stock `ghcr.io/parkervcp/steamcmd:debian` (rcon + steam libs);
  no custom image needed — the
  [palworld-admin](https://github.com/Soul-Returns/pelican-plugin-palworld-admin)
  panel plugin (v1.6.0+) does its Pal exports in the browser.
- Official REST API wired up (`REST_API_ENABLED` / `REST_API_PORT` variables,
  applied via the config parser; needs a TCP allocation for the port).
- Hardened installer (HTTP/1.1 + retry/resume against GHCR stream resets,
  per-layer progress) and input guards (`" ( )` are rejected in server
  name/description — they corrupt the config parser).
- Console/stop run over RCON; startup-done marker on the breakpad line.

Requirements per server: `ADMIN_PASSWORD` set (use something long and random -
it is also the REST API credential), game port UDP allocation, and a TCP
allocation matching `REST_API_PORT` if the panel plugin is used.

## icarus

`icarus/egg-icarus--dedicated-fixed.json` — Icarus dedicated server (PTDL_v2).

## rust

`rust/egg-rust-carbon.yaml` — Rust with the Carbon modding framework.
