# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Removed

- The `client` variant. Every variant (the default, `26_60` and `26_51`) has a server and a client build, and lip and LeviLauncher install the one for the side they install on: on a client, install the default variant (or `26_51`).

## [0.2.0-beta.1] - 2026-11-11

First beta of 0.2.0. Please report anything that behaves differently from 0.1.1.

### Added

- Support for **Minecraft 1.26.60** with LeviLamina 26.60.x, on dedicated servers and in single-player; it is now the version NaturesLodestone is installed as by default. Minecraft 1.26.51 with LeviLamina 26.51.x is still supported and gets its own builds of every release.
  - NaturesLodestone is compiled against one LeviLamina line at a time, so each release now ships one download per side and line: `natureslodestone-server-win-x64-ll26.60.zip`, `natureslodestone-client-win-x64-ll26.60.zip`, `natureslodestone-server-win-x64-ll26.51.zip` and `natureslodestone-client-win-x64-ll26.51.zip`.

### Changed

- The package is now described by `lip.toml` of format 4 instead of `tooth.json`, so installing it needs [lip](https://github.com/LiteLDev/lip) **0.40.0** or later, or a LeviLauncher that reads it. Older releases stay installable with older lip.
- Each supported LeviLamina line is a variant named after it, with the server and the client build: `26_60` for 26.60.x and `26_51` for 26.51.x. The default variant (on a server and on a client) and `client`, the variant earlier releases installed the client mod as, are the newest line, so `lip update` and LeviLauncher keep working and move you to 26.60.
  - To stay on Minecraft 1.26.51, install `github.com/MiracleForest/NaturesLodestone#26_51`, or pick the `26_51` variant on the mod's page in LeviLauncher.
  - The config in `mods/NaturesLodestone/config/` (`plugins/NaturesLodestone/config/` on a LeviLamina 26.51 server) is kept.
- On a LeviLamina 26.60 server the mod is installed into `mods/NaturesLodestone/`, next to LeviLamina, which 26.60 moved there from `plugins/`. On a 26.51 server it stays in `plugins/NaturesLodestone/`.
- The downloads are now lip mod archives: the files sit at the root of the ZIP instead of inside a `NaturesLodestone/` folder. Extract one into a `NaturesLodestone` folder, not next to it.

## [0.1.1] - 2026-11-01

### Changed

- The repository moved to [MiracleForest/NaturesLodestone](https://github.com/MiracleForest/NaturesLodestone); links point there now.
- The lip package is now `github.com/MiracleForest/NaturesLodestone` (was `github.com/LiteLDev/NaturesLodestone`); installing by the new path needs lip 0.35.0 or later. With lip 0.36.0 or later, `lip update` moves an existing install to the new path. With older lip, run `lip uninstall github.com/LiteLDev/NaturesLodestone` (`#client` on a client), then `lip install github.com/MiracleForest/NaturesLodestone` (`#client` on a client). Either way the config in `plugins/NaturesLodestone/config/` (server) or `mods/NaturesLodestone/config/` (client) is kept. Older versions stay installable at their old path.

## [0.1.0] - 2026-10-29

First release: Nature's Compass 3.3.1 for Bedrock 1.26.51 (LeviLamina 26.51), on dedicated servers and in single-player.

### Added

- The Nature's Compass item (`natureslodestone:naturescompass`) with its crafting recipe (saplings, logs and a compass) and the repair recipe (Nature's Compass + compass).
- Using the compass: right-click (in the air or on a block) opens Select Biome, sneak + right-click resets the compass; one action per click.
- The Select Biome screen: search box (`$tags`, `@source` filters), Sort By (Name, Source, Tags, Dimension, Levels, Rainfall, Temperature), biome rows with dimension / tags / source lines and XP level costs, greyed rows the player can't afford, selection (a second click searches), Search, Search for Next, Teleport and Cancel. It is drawn in Nature's Compass' layout by the resource pack; players without the pack get a data-driven form with the same contents.
- The biome catalog: every biome the world can generate, with its dimensions, Nature's Compass tags, source, temperature, rainfall and XP cost; the biome blacklist and per-biome XP costs; English and Chinese names for every biome and dimension.
- Biome searches as in Nature's Compass: a square spiral over the world generator's biomes (no chunk loading) in the Overworld, the Nether and the End, within a per-tick time budget, with radius progress, Not Found limits and "Search for Next" skipping biome patches connected to earlier finds. One search per compass.
- Search and Search for Next take the configured XP levels (not in creative) and play the click sound; a broken compass can't search.
- Teleport to a found biome (operators or cheats, `allowTeleport`) at a safe height, also into chunks that are not loaded yet.
- Durability (`compassDurability`): successful searches wear the compass down, shown on its durability bar.
- The Nature's Compass HUD: status, biome, radius, coordinates, distance and samples of the held compass, drawn natively by the client mod and sent by the server to vanilla clients (rendered by the resource pack), on the left or right side (`overlaySide`, `overlayLineOffset`, `displayWithChatOpen`).
- The animated compass needle (client build): 32 needle frames of original pixel art that point at the found biome, or at the world spawn before a search, turning with the player like a vanilla compass and spinning when dropped. Vanilla clients see the plain icon.
- The `naturescompass.json` and `naturescompass_client.json` configuration files with every Nature's Compass setting.
- The behaviour spec of the port in `docs/SPEC.md`, and usage, configuration and the differences from Nature's Compass in the README.

[Unreleased]: https://github.com/MiracleForest/NaturesLodestone/compare/v0.2.0-beta.1...HEAD
[0.2.0-beta.1]: https://github.com/MiracleForest/NaturesLodestone/compare/v0.1.1...v0.2.0-beta.1
[0.1.1]: https://github.com/MiracleForest/NaturesLodestone/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/MiracleForest/NaturesLodestone/releases/tag/v0.1.0
