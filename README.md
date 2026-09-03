# Minecraft Proxy (Velocity + Geyser + Floodgate) on Docker Compose

[![Deployment Verification](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/deployment-verification.yml/badge.svg?branch=main)](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/deployment-verification.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository deploys a **Velocity** Minecraft proxy with **Geyser** and **Floodgate**: so both Java Edition and Bedrock Edition players (console, mobile, Windows) can join the servers behind it. Pairs with [minecraft-server-docker-compose](https://github.com/heyvaldemar/minecraft-server-docker-compose) as the backend.

## Getting started

```bash
# 1. Clone
git clone https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose
cd minecraft-server-proxy-docker-compose

# 2. Point the proxy at your backend server(s)
$EDITOR velocity.toml   # [servers] section; host.docker.internal reaches the host

# 3. Deploy (defaults work, .env only for overrides)
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

- **Proxy answers on the wrong port.** Your `velocity.toml` isn't being applied. Keep the `./velocity.toml:/config/velocity.toml:ro` mount exactly as shipped. Note the image copies it into `/server` only if `/server/velocity.toml` doesn't already exist; delete `minecraft-server-proxy-data/velocity.toml` after config changes.
- **Bedrock players can't join.** UDP 19132 must be open in your firewall; Geyser uses UDP, not TCP.
- **Backend refuses proxied players.** Configure [player-info-forwarding](https://docs.papermc.io/velocity/player-information-forwarding) on the backend with the generated `forwarding.secret`.

## Supply chain trust

The proxy image [`itzg/mc-proxy`](https://hub.docker.com/r/itzg/mc-proxy) is pinned to `tag@sha256:<digest>` as an interpolation default in the compose `x-images` block. Earlier revisions deployed from `latest`. `git pull` alone delivers the tested version.

The daily `check-pin-freshness` CI job re-resolves the pin and compares it against the latest itzg release. GitHub Actions are pinned by commit SHA; Dependabot keeps those fresh.

## Production checklist

- [ ] **Keep online-mode on** (default in the shipped `velocity.toml`) unless you fully understand the consequences.
- [ ] **Set up player-info forwarding** to the backend with the generated `forwarding.secret`. Never disable it on an internet-facing proxy.
- [ ] **Open 25565/tcp and 19132/udp** in the firewall; nothing else needs to be public.
- [ ] **Back up `minecraft-server-proxy-data/`**: it holds `forwarding.secret` and plugin state.

## Unattended updates

Releases are the update channel: a tag is cut only after CI has built the pinned images, booted the full stack, and passed the smoke tests. `update.sh` moves a deployment to the newest tag and nothing else:

```bash
./update.sh --dry-run   # show what would be applied
./update.sh             # update within the current major and redeploy
```

Put it on a timer for hands-off minor/patch updates:

```bash
# crontab -e
17 5 * * *  /opt/minecraft-server-proxy-docker-compose/update.sh >> /var/log/minecraft-server-proxy-update.log 2>&1
```

The script refuses to cross a MAJOR template version on its own: majors are breaking by definition and their release notes exist to be read. After reading them, `./update.sh --allow-major` performs the jump. It also refuses to touch a checkout with local modifications: your customization belongs in `.env`, which updates never overwrite.

This is deliberately a host-side script and not a container in the stack: an in-stack updater needs the Docker socket (root on the host) and turns "someone pushed to a repo" into "someone deployed to your machine" with no operator in the loop. A cron job under your own user updates only to tagged, CI-verified states and leaves the trust boundary where it was.

## Resource limits

Every service carries memory and CPU limits plus reservations as compose-level defaults, the same values CI boots the stack under. Override any of them in `.env` (the knobs and their defaults are listed in `.env.example`, e.g. `TRAEFIK_MEMORY_LIMIT=512m`) and the override survives every `git pull`. If a service is OOM-killed under real load, `docker inspect <container> --format '{{.State.OOMKilled}}'` says so; raise its `_MEMORY_LIMIT` and recreate.

## Backups

The proxy is stateless: `velocity.toml` and the `plugins` directory next to the compose file are the configuration, and `minecraft-server-proxy-data` holds only the proxy's runtime files (forwarding secret, logs). Keep the configuration in your own git repository; the players' worlds live on the backend servers, which carry their own backups.

## Container hardening

Every service runs with `security_opt: no-new-privileges:true`, so a process cannot gain privileges through setuid binaries even if it escapes its initial capability set. Infrastructure containers (the reverse proxy, databases, caches, backups) run with `cap_drop: [ALL]` and add back only what their entrypoints need: `NET_BIND_SERVICE` for Traefik to bind :80/:443, `CHOWN`/`SETUID`/`SETGID` (and friends) for database images to own their data directory and drop to their service user. Application containers keep the default capability set on purpose: upstream images assume it, and a wrong guess there is a boot loop in production rather than a hardening win. CI boots the stack under exactly these settings on every push, so what ships is what was tested.

## Testing

The [Deployment Verification](https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/actions/workflows/deployment-verification.yml?query=branch%3Amain) workflow runs on every push, pull request, and every day at 06:00 UTC: actionlint, a Trivy scan of the pinned image, the weekly freshness check, and a deploy-and-test job that boots the proxy and requires Velocity to load Geyser/Floodgate and listen on the advertised port.

---

## About the maintainer

<div align="center">

**Maintained by [Vladimir Mikhalev](https://github.com/heyvaldemar)** · Docker Captain · IBM Champion · AWS Community Builder

[YouTube](https://www.youtube.com/channel/UCf85kQ0u1sYTTTyKVpxrlyQ?sub_confirmation=1) · [Blog](https://heyvaldemar.com) · [LinkedIn](https://www.linkedin.com/in/heyvaldemar/)

</div>
