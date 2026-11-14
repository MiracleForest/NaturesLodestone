# Contributing to NaturesLodestone

Thanks for helping! Bug reports, ideas and pull requests are all welcome.

## Reporting bugs and requesting features

Use the [issue templates](https://github.com/MiracleForest/NaturesLodestone/issues/new/choose). For bugs, include:

- the NaturesLodestone version;
- the LeviLamina version;
- the Minecraft version;
- whether it happens on a dedicated server, in a local world, or with a vanilla client;
- steps to reproduce;
- the relevant part of the server console or `logs/latest.log`.

## Development setup

See [Development](#development) below. [AGENTS.md](AGENTS.md) is the developer guide. It covers the layout of the code, the feature units and the contracts between them, the registration macros and the rules for working with Bedrock internals. Please read it before changing anything under `src/`.

## Pull requests

1. Fork the repository and create a branch from `main`.
2. Keep each pull request focused on one change. Describe what it does and how you tested it.
3. Follow the existing style: C++20, the provided `.clang-format` and `.editorconfig`, English in code and comments.
4. User-facing text goes through `nl::tr` / `nl::trFor` / `nl::client::ctr` with entries in **both** `assets/lang/en_US/` and `assets/lang/zh_CN/`.
5. Logic in `src/core` must stay free of game headers and come with unit tests in `tests/`:
   `xmake build -y nl-tests; xmake run nl-tests`.
6. Gameplay changes come with selftests (`NL_REGISTER_SELFTEST`) and an end-to-end scenario in [tools/e2e](tools/e2e/README.md). Run the relevant scenarios, and `tools/e2e/server/run.ps1 -Scenario full` for anything that touches shared code.
7. Both platforms must build: `pwsh tools/build.ps1 -Target all -Tests`. CI must pass before a pull request is merged.
8. NaturesLodestone is licensed CC BY-NC-SA 4.0. Code adapted from third-party sources must come only from the sources and versions listed in [NOTICE](NOTICE) and keep that attribution intact; do not add textures, models or sounds from them.

Commit messages use the imperative mood ("Add the biome search cooldown", not "Added …"). Update [CHANGELOG.md](CHANGELOG.md) under *Unreleased* for user-visible changes.

## Development

### Building

You need Windows x64, [xmake](https://xmake.io), Visual Studio Build Tools with MSVC 14.4x or newer, and LLVM (`clang-cl`).

```powershell
git config --global core.longpaths true
pwsh tools/build.ps1 -Target all -Tests                    # bin/server/NaturesLodestone/ and bin/client/NaturesLodestone/, plus unit tests
pwsh tools/build.ps1 -Target client                        # one platform only
pwsh tools/build.ps1 -Target all -LeviLamina 26.51.7       # another LeviLamina line
```

One code base builds both the dedicated-server mod and the client mod (`--target_type=server|client`). The mod version is the `version` in `manifest.json`.

It is compiled against one LeviLamina line at a time, chosen with `-LeviLamina` (xmake `--levilamina`). Each supported line has a lip variant of the same name and a lock file in `locks/`, which pins the packages of that line; `tools/build.ps1` copies it to `xmake-requires.lock` (which is generated, and not tracked) before configuring:

| Line | Variant | Lock file | Build with |
| --- | --- | --- | --- |
| LeviLamina 26.60.x (Minecraft 1.26.60) | `26_60` | `locks/26_60.lock` | `-LeviLamina 26.60.6` (the default) |
| LeviLamina 26.51.x (Minecraft 1.26.51) | `26_51` | `locks/26_51.lock` | `-LeviLamina 26.51.7` |

The build defines `NL_LL_VERSION` as the line it is building for (`2651`, `2660`). Where a LeviLamina API differs between lines, guard both paths with it rather than dropping the older one, and keep the guard as small as possible:

```cpp
#if NL_LL_VERSION >= 2660
    // 26.60 and later
#else
    // 26.51
#endif
```

To add a line, add its row above: a `locks/<variant>.lock` (configure once with the new `--levilamina` for both target types and copy the generated `xmake-requires.lock`), a variant in `lip.toml`, and a matrix entry in `.github/workflows/build.yml` and `.github/workflows/release.yml`.

### Tests

- Unit tests (`src/core`): `xmake build -y nl-tests; xmake run nl-tests`.
- In-game selftests: `/natureslodestone selftest ["glob"]` (operator or server console).
- End-to-end: [tools/e2e](tools/e2e/README.md) runs the real server and client, headless through [LeviHeadlessInput](https://github.com/LiteLDev/BeDD). A `showcase` client scenario re-shoots the screenshots and videos in `docs/showcase/` (listed in `showcase.json` for the LeviMC Mods gallery); encode its PNGs and frames with ffmpeg (webp for images, mp4 + animated webp for clips).

### How it works

- [AGENTS.md](AGENTS.md) is the developer guide: the layout of the code, the feature units and the contracts between them, the registration macros and the rules for working with Bedrock internals. Please read it before changing anything under `src/`.
- [docs/SPEC.md](docs/SPEC.md) describes the behaviour NaturesLodestone implements (blocks, items, mechanics, every screen, configuration, commands, strings).
- NaturesLodestone is licensed CC BY-NC-SA 4.0; keep the attribution in [NOTICE](NOTICE) up to date. Its textures are original pixel art drawn by `tools/textures/*.ps1`; do not add textures, models or sounds from third-party sources.

### Maintainers

[@futrime (Zijian Zhang)](https://github.com/futrime)

## Releasing (maintainers)

1. In [CHANGELOG.md](CHANGELOG.md), move the *Unreleased* entries under a new `## [x.y.z] - YYYY-MM-DD` section and update the links at the bottom. Set `version` to `x.y.z` in [lip.toml](lip.toml) and in [manifest.json](manifest.json) (what the mod reports in game).
2. Merge that into `main`. Then run every server and client e2e scenario against `main`.
3. Tag the merge commit `vx.y.z` and push the tag. The *Release* workflow:
   - builds both platforms for every supported LeviLamina line and runs the unit tests;
   - checks that the mod manifest and `lip.toml` carry the tag's version;
   - publishes `natureslodestone-<server|client>-win-x64-ll<line>.zip` for every side and line (packed for lip's `mcmod` asset, with `manifest.json` at the root of the ZIP), with that version's CHANGELOG section as the release notes; a version with a `-` in it, such as `0.2.0-beta.1`, is published as a prerelease.

## License

By contributing you agree that your contributions are licensed under [CC BY-NC-SA 4.0](LICENSE), the license of this project (see [NOTICE](NOTICE)).
