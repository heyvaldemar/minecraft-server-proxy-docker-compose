# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

_(no unreleased changes yet)_

## [1.0.0] - 2026-09-01

### Fixed (the shipped config was silently ignored)

- **`velocity.toml` was mounted over the `/config` directory itself**,
  which the itzg image ignores — Velocity started with its defaults and
  listened on 25577 while the compose published 25565, so the proxy
  never answered on the advertised port. The file is now mounted as
  `/config/velocity.toml`, which the image copies into `/server` on
  startup (the same mount works on every OS; the old per-OS comment
  block is gone).

### Changed

- **`itzg/mc-proxy:latest` → pinned `2026.8.2` by `tag@sha256:digest`**
  in the compose `x-images` block; all variables have working defaults,
  so an empty `.env` boots the stack.

### Added

- **Deployment Verification workflow**: actionlint; Trivy scan of the
  pinned image; weekly `check-pin-freshness` (digest drift + itzg
  release lag); and a deploy-and-test job that boots the proxy and
  requires Velocity to load Geyser/Floodgate and listen on 25565.
- `.gitignore` entries for `.env` and the runtime server data.

[Unreleased]: https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/heyvaldemar/minecraft-server-proxy-docker-compose/releases/tag/v1.0.0
