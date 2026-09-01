# Minecraft Proxy (Velocity + Geyser + Floodgate) — Docker Compose

[![Deployment Verification](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/deployment-verification.yml/badge.svg?branch=main)](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/deployment-verification.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository deploys a **Velocity** Minecraft proxy with **Geyser** and **Floodgate** — so both Java Edition and Bedrock Edition players (console, mobile, Windows) can join the servers behind it. Pairs with [minecraft-server-docker-compose](https://github.com/heyvaldemar/minecraft-server-docker-compose) as the backend.

## Getting started

```bash
# 1. Clone
git clone https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose
cd minecraft-server-proxy-docker-compose

# 2. Point the proxy at your backend server(s)
$EDITOR velocity.toml   # [servers] section; host.docker.internal reaches the host

# 3. Deploy (defaults work — .env only for overrides)
docker compose -f minecraft-server-proxy-docker-compose.yml -p minecraft-proxy up -d
```

Java players connect to port **25565** (TCP), Bedrock players to **19132** (UDP). Geyser and Floodgate are downloaded automatically on first start.

### What success looks like

```bash
docker compose -f minecraft-server-proxy-docker-compose.yml -p minecraft-proxy logs | grep -E "Listening|Geyser"
# [INFO]: Listening on /[0:0:0:0:0:0:0:0]:25565
# [INFO] [geyser]: Started Geyser on UDP port 19132
```

### Common first-deploy issues

- **Proxy answers on the wrong port.** Your `velocity.toml` isn't being applied — keep the `./velocity.toml:/config/velocity.toml:ro` mount exactly as shipped. Note the image copies it into `/server` only if `/server/velocity.toml` doesn't already exist; delete `minecraft-server-proxy-data/velocity.toml` after config changes.
- **Bedrock players can't join.** UDP 19132 must be open in your firewall; Geyser uses UDP, not TCP.
- **Backend refuses proxied players.** Configure [player-info-forwarding](https://docs.papermc.io/velocity/player-information-forwarding) on the backend with the generated `forwarding.secret`.

## Supply chain trust

The proxy image [`itzg/mc-proxy`](https://hub.docker.com/r/itzg/mc-proxy) is pinned to `tag@sha256:<digest>` as an interpolation default in the compose `x-images` block — earlier revisions deployed from `latest`. `git pull` alone delivers the tested version.

The weekly `check-pin-freshness` CI job re-resolves the pin and compares it against the latest itzg release. GitHub Actions are pinned by commit SHA; Dependabot keeps those fresh.

## Production checklist

- [ ] **Keep online-mode on** (default in the shipped `velocity.toml`) unless you fully understand the consequences.
- [ ] **Set up player-info forwarding** to the backend with the generated `forwarding.secret` — never disable it on an internet-facing proxy.
- [ ] **Open 25565/tcp and 19132/udp** in the firewall; nothing else needs to be public.
- [ ] **Back up `minecraft-server-proxy-data/`** — it holds `forwarding.secret` and plugin state.

## Testing

The [Deployment Verification](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/deployment-verification.yml?query=branch%3Amain) workflow runs on every push, pull request, and every Monday at 06:00 UTC: actionlint, a Trivy scan of the pinned image, the weekly freshness check, and a deploy-and-test job that boots the proxy and requires Velocity to load Geyser/Floodgate and listen on the advertised port.

---

## About the maintainer

<div align="center">

**Maintained by [Vladimir Mikhalev](https://github.com/heyvaldemar)** — Docker Captain · IBM Champion · AWS Community Builder

[YouTube](https://www.youtube.com/channel/UCf85kQ0u1sYTTTyKVpxrlyQ?sub_confirmation=1) · [Blog](https://heyvaldemar.com) · [LinkedIn](https://www.linkedin.com/in/heyvaldemar/)

</div>
