# G9650 Android 16

Android 16 / LineageOS porting project for Samsung Galaxy S9+ Snapdragon variant:

- **Device:** Samsung Galaxy S9+ SM-G9650
- **Device codename:** `star2qltechn`
- **Platform:** Qualcomm Snapdragon 845 (SDM845)
- **Goal:** Build a maintainable Android 16 ROM and validate real-device functionality, not merely achieve a successful compile or boot.

> **Status:** Initial project scaffold. No Android 16 build has been verified from this repository yet.

## Development strategy

1. **Re-establish the known-good baseline.** Locate and pin the exact LineageOS 19.1 ROM/build and matching device tree, common tree, kernel, vendor files, firmware, and recovery used for the previous G9650 test.
2. **Document the current device state.** Record bootloader/firmware versions, partition layout, recovery version, and backup/recovery path before any experimental flash.
3. **Audit source compatibility.** Compare the 19.1 base against newer community SDM845/Samsung sources. Do not assume that an Android 13-era device tree or kernel can be reused unchanged for Android 16.
4. **Prepare reproducible build configuration.** Keep manifests, patches, scripts, and CI configuration here; do not commit the full Android source tree or proprietary firmware blobs.
5. **Compile incrementally.** First validate repository sync and manifest resolution, then device configuration, kernel integration, image generation, and finally boot/functional testing.
6. **Validate on hardware.** Maintain a test log for boot, telephony/RIL, LTE registration, Bluetooth, Wi-Fi, audio, camera, sensors, GPS, charging, sleep/wake, encryption, and recovery.

## Known test observations from the previous session

These are user-reported observations and must be reproduced against a pinned build before being treated as confirmed project status:

- LineageOS 19.1 was tested on the G9650.
- SIM/network registration was unreliable in the default network mode; LTE-only mode could register on China Unicom.
- Bluetooth was reported unavailable on one LineageOS 19.1 setup.

The exact build fingerprint, ZIP filename/hash, firmware version, logs, and test conditions still need to be recorded. Do not infer that every LineageOS 19.1 build has these same issues.

## Repository layout (planned)

```text
.
├── README.md
├── docs/
│   ├── baseline.md          # Exact reference build and firmware
│   ├── compatibility.md     # Device-tree/kernel/vendor audit
│   ├── test-plan.md         # Hardware test checklist
│   └── build-notes.md       # Reproducible build notes
├── local_manifests/
│   └── README.md            # Planned repo manifest overlays
├── patches/
│   └── README.md            # Tracked source patches only
└── .github/
    └── workflows/
        └── validate-config.yml
```

## Build infrastructure notes

GitHub Actions can validate the repository configuration and coordinate builds. A full Android build needs substantially more disk and memory than a standard GitHub-hosted runner provides. Before enabling a full build, choose a supported larger runner or a secured cloud self-hosted runner and confirm storage, build time, artifact retention, and cost.

Never put access tokens, signing keys, private credentials, or proprietary firmware in the repository or build logs.

## Definition of done

A candidate is not considered daily-use-ready until it has:

- A reproducible build tied to a documented source revision.
- A bootable image and a verified way to recover the device if boot fails.
- Successful basic hardware tests, with known limitations clearly documented.
- No unresolved critical failures in telephony, connectivity, charging, or sleep/wake.
- Published checksums and build/test notes for each release.

## Next actions

See [baseline.md](docs/baseline.md), [compatibility.md](docs/compatibility.md), and [test-plan.md](docs/test-plan.md) as they are added.
