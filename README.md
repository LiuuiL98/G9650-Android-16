# G9650 Android 16

Community porting project targeting Android 16 / LineageOS for the Samsung Galaxy S9+ Snapdragon variant.

- **Device:** Samsung Galaxy S9+ SM-G9650
- **Device codename:** `star2qltechn`
- **Platform:** Qualcomm Snapdragon 845 (SDM845)
- **Android 16 target branch:** LineageOS `lineage-23.2`
- **Goal:** A reproducible, maintainable ROM validated on real hardware—not merely a successful compile or first boot.

> **Current status:** Source reconnaissance and reference manifest are in progress. No Android 16 image has been built or verified by this repository.

## Development strategy

1. **Use the published LineageOS 19.1 release as a historical behavior reference.** The exact-S9+ asset and caveats are documented in [known-rom reference](docs/known-good-rom-reference.md).
2. **Lock a coherent intermediate source set.** A pinned LineageOS 20 / Android 13 reference manifest has been added to help validate the device-specific tree, common tree, kernel and vendor integration. This is an integration checkpoint only, not the final Android 16 target.
3. **Create the Android 16 port.** Start from LineageOS `lineage-23.2`, adapt the SDM845/Samsung sources and resolve build, VINTF, SELinux, init, radio, kernel, vendor/HAL and image-packaging issues.
4. **Build reproducibly.** Keep small local manifests, patch sets, automation and logs here; do not commit the full Android source tree or proprietary firmware blobs.
5. **Validate on hardware.** Track boot, telephony/RIL, network registration, Bluetooth, Wi-Fi, audio, camera, sensors, GPS, charging, sleep/wake, encryption and recovery.

## Current documents

- [Roadmap and milestone gates](docs/roadmap.md)
- [LineageOS 19.1 source audit](docs/lineage-19.1-source-audit.md)
- [Published LineageOS 19.1 reference image](docs/known-good-rom-reference.md)
- [Pinned LineageOS 20 reference source set](docs/reference-source-set.md)
- [Android 16 compatibility gap analysis](docs/android-16-gap-analysis.md)
- [Device/common/kernel/vendor compatibility audit](docs/compatibility.md)
- [Baseline capture checklist](docs/baseline.md)
- [Hardware validation plan](docs/test-plan.md)
- [Reference manifest XML](local_manifests/lineage-20-reference.xml)

## Prior device observations

These are user-reported observations from an earlier LineageOS 19.1 setup and must be reproduced against an exact, pinned build before being treated as confirmed project-wide issues:

- SIM/network registration was unreliable in the default network mode; LTE-only mode could register on China Unicom.
- Bluetooth was reported unavailable on one setup.

The exact ROM ZIP, SHA-256, build fingerprint, firmware/baseband and diagnostic logs should be captured where available. Do not infer that all builds have the same issues.

## Build infrastructure

GitHub Actions is currently for configuration validation. A full Android source build requires far more storage and memory than a standard GitHub-hosted runner provides. Before enabling a full build, choose a supported larger runner or a secured cloud self-hosted runner and confirm resource needs and costs.

Never expose a self-hosted runner to untrusted pull requests. Never commit access tokens, signing keys, private credentials, or proprietary blobs without clear redistribution permission.

## Definition of done

A build is not considered daily-use-ready until it has a reproducible source revision, bootable and recoverable images, documented checksums, a completed hardware-test report, and no unresolved critical faults in telephony, connectivity, charging, or sleep/wake.
