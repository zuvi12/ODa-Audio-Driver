# ODa Audio Driver

Open-source virtual audio driver for the [ODa](https://github.com/zuvi12) audio mixer application.

Creates virtual audio endpoints that appear in Windows Sound Settings, allowing per-app audio routing similar to SteelSeries Sonar / VoiceMeeter Banana.

## Endpoints created (planned)

| Name | Type | Purpose |
|---|---|---|
| `ODa - Game` | Render (output) | Audio from games |
| `ODa - Chat` | Render (output) | Voice chat apps (Discord, Teams, Zoom) |
| `ODa - Media` | Render (output) | Music / browser / media apps |
| `ODa - Aux` | Render (output) | Anything else |
| `ODa - Mic` | Capture (input) | Virtual mic, fed by the ODa app (with optional noise suppression) |

The user picks ODa endpoints in Windows Settings → Sound → Volume mixer (per-app routing). The ODa app reads from each virtual endpoint, mixes per channel, and outputs to a physical device.

## Build

The driver is built and signed automatically by GitHub Actions on every push to `main`. Signed binaries are uploaded as workflow artifacts and as GitHub Releases.

Code signing is provided **for free** by the [SignPath Foundation](https://signpath.org/) for qualifying open-source projects. Certificate stays with SignPath; binaries are signed in their isolated infrastructure.

To build locally (requires Windows + Visual Studio 2022 + WDK 10.0.26100+ with VS Integration):

```cmd
build.bat
```

Output goes to `x64\Release\package\`.

## Project structure

```
ODa-Audio-Driver/
├── Source/
│   ├── Main/        - DriverEntry, miniport implementations, INF source
│   ├── Filters/     - Per-endpoint topology and wave tables
│   ├── Inc/         - Shared headers (endpoints, common, definitions)
│   └── Utilities/   - Helpers (savedata, hw, ToneGenerator, kshelper)
├── Package/         - INF/CAT packaging project
├── .github/workflows/
│   ├── ODA-compile-and-sign.yml   - Production: build + SignPath sign + Inno installer
│   └── ODA-compile.yml            - PR builds: compile only, no signing
└── ODaAudioDriver.sln
```

## Status

This is a stage-1 fork from [VirtualDrivers/Virtual-Audio-Driver](https://github.com/VirtualDrivers/Virtual-Audio-Driver) (MIT) with ODa branding applied. Multi-endpoint support (5 named cables) is the next milestone.

## License

MIT for the original ODa modifications. Portions derived from Microsoft Windows Driver Samples (MS-PL) and VirtualDrivers/Virtual-Audio-Driver (MIT). Full attribution in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Acknowledgments

- [Microsoft Windows Driver Samples — Sysvad](https://github.com/microsoft/Windows-driver-samples/tree/main/audio/sysvad) — the original Simple Audio Sample base
- [VirtualDrivers/Virtual-Audio-Driver](https://github.com/VirtualDrivers/Virtual-Audio-Driver) by MikeTheTech — modernized base + signing pipeline
- [SignPath Foundation](https://signpath.org/) — free code signing for OSS projects
