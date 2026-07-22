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
from GHCR and extracted; update = panel "Reinstall", world data preserved).

Highlights:

- Runtime image: [`ghcr.io/soul-returns/yolks:palworld`](https://github.com/Soul-Returns/yolks)
  — includes the *paltools* watcher used by the
  [palworld-admin](https://github.com/Soul-Returns/pelican-plugin-palworld-admin)
  panel plugin (filtered Pal exports). A plain steamcmd image is available as a
  no-paltools fallback.
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
