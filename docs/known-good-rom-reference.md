# Known ROM reference: LineageOS 19.1 for G9650

Checked: 2026-10-13

## Published release

- Project: [CarlosJRay/LineageOs_sdm845](https://github.com/CarlosJRay/LineageOs_sdm845)
- Release: [19.1 — LineageOs 19.1 download](https://github.com/CarlosJRay/LineageOs_sdm845/releases/tag/19.1)
- Published: 2022-10-17; the S9+ asset was uploaded on 2022-10-25.
- Release note: maintainer says this is their first build and asks users to test at their own risk. Codenames listed: `starqltechn`, `star2qltechn`, `crownqltechn`.
- Status: unofficial historical release, not proof of current security support or complete hardware stability.

## Exact SM-G9650 / star2qltechn asset

**Download:** [lineage-19.1-20221025-UNOFFICIAL-star2qltechn.zip](https://github.com/CarlosJRay/LineageOs_sdm845/releases/download/19.1/lineage-19.1-20221025-UNOFFICIAL-star2qltechn.zip)

- GitHub asset ID: `82216131`
- Size: 676,505,274 bytes (about 645 MiB)
- SHA-256 / digest: not published in the release metadata checked
- Download count reported by GitHub at audit time: 1,589
- Build date in filename: 2022-10-25

Do not use the `starqltechn` ZIP, which is for the S9 (SM-G9600), on the S9+ (SM-G9650). The target asset is the `star2qltechn` ZIP.

## Source provenance clues from the release

- The release body credits `matebuteler` for the device tree.
- The exact [star2qltechn device tree](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1) README says it is a bring-up of Nyoutaaa's tree from dotOS to LineageOS.
- Device-tree revision: `06e450924ba754d141b1b8df04259931f175c30d`. Product files identify `star2qltechn` / `SM-G9650`, and `BoardConfig.mk` uses `star2qlte_chn_open_defconfig`.
- Its `lineage.dependencies` names `matebuteler/android_device_samsung_sdm845-common`, but that repository currently exposes a `lineage-18.1` branch, so the exact 19.1 dependency revision still needs to be recovered.
- The release includes source changelog link: [Sasukezin678/LineageOs_starqltechn compare Los...19.1](https://github.com/Sasukezin678/LineageOs_starqltechn/compare/Los...19.1).

These clues narrow down the original source ancestry but do not yet establish a complete, reproducible manifest. The published release asset is a direct baseline for behaviour comparison; source reconstruction remains a separate task.

## Next reproducibility work

1. If the ZIP previously tested by the user matches the asset above, record its local SHA-256 and confirm the build fingerprint after installation.
2. Use this image as a historical device-behavior reference and capture kernel version, baseband, Bluetooth logs, and radio logs.
3. Reconstruct the source dependencies matching the release rather than combining similar forks arbitrarily.
4. After the baseline manifest can be synced and built reproducibly, port forward to Android 16.

Do not commit ROM binaries, proprietary vendor dumps, personal device identifiers or access credentials into this source repository.
