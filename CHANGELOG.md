# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-11-14

First release: a biome-finding compass for Minecraft: Bedrock Edition on Windows (x64), for dedicated servers and single-player worlds.

### Added

- Support for **Minecraft 1.26.60** (LeviLamina 26.60.x) and **Minecraft 1.26.51** (LeviLamina 26.51.x), each with a server and a client build: `natureslodestone-server-win-x64-ll26.60.zip`, `natureslodestone-client-win-x64-ll26.60.zip`, `natureslodestone-server-win-x64-ll26.51.zip` and `natureslodestone-client-win-x64-ll26.51.zip`.
- Installation with [lip](https://github.com/LiteLDev/lip) 0.40.0 or later (`lip install github.com/MiracleForest/NaturesLodestone`, or `#26_51` for LeviLamina 26.51) or through LeviLauncher. The default variant is the newest line (26.60); each line is also a variant of its own (`26_60`, `26_51`). The mod lives in `mods/NaturesLodestone/` (`plugins/NaturesLodestone/` on a LeviLamina 26.51 server).
- The Biome Compass item (`natureslodestone:biome_compass`) with its crafting recipe (a compass, logs and four saplings of one kind) and a repair recipe (Biome Compass + compass).
- Using the compass: right-click (in the air or on a block) opens Select Biome; sneak + right-click stops the search and resets the compass.
- The Select Biome screen: a search box (with `$tag` and `@source` filters), Sort By (Name, Source, Tags, Dimension, Levels, Rainfall, Temperature), biome rows with dimension, tags, source and XP cost, greyed-out rows the player can't afford, Search, Search for Next, Teleport and Cancel. The resource pack draws it as a custom screen; players without the pack get a form with the same contents.
- The biome catalog: every biome the world can generate, with its dimensions, tags, source, temperature, rainfall and XP cost; a biome blacklist and per-biome XP costs; English and Chinese names for every biome and dimension.
- Biome searches in the Overworld, the Nether and the End: a spiral over the world generator's biomes (no chunk loading) that runs a little every tick, with radius progress, Found / Not Found results, and "Search for Next" to find further patches not connected to earlier finds. One search per compass; several players can search at once.
- XP costs for searches (not in creative) and teleporting to a found biome at a safe height (operators or worlds with cheats, `allowTeleport`), including into chunks that are not loaded yet.
- Compass durability (`compassDurability`): successful searches wear the compass down, shown on its durability bar; a broken compass can't search.
- The compass HUD: status, biome, radius, coordinates, distance and samples, drawn by the client mod, or by the server's resource pack for vanilla players, on the left or right side of the screen.
- The animated compass needle (client mod): points at the found biome, or at the world spawn before a search, and turns with the player like a vanilla compass. Vanilla players see a static icon.
- The `/natureslodestone` command with `reload` (configuration and translations) and `selftest` for operators and the console.
- Configuration in `config/mod.json`, `config/natureslodestone.json` and `config/natureslodestone_client.json`, created with defaults on first start.
- Usage and configuration in the README, and the full behaviour in `docs/SPEC.md`.

[Unreleased]: https://github.com/MiracleForest/NaturesLodestone/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/MiracleForest/NaturesLodestone/releases/tag/v0.1.0
