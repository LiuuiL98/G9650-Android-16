# Known ROM reference: LineageOS 19.1 for G9650

Checked: 2026-10-13

## Published release

- Project: [CarlosJRay/LineageOs_sdm845](https://github.com/CarlosJRay/LineageOs_sdm845)
- Release: [19.1 — LineageOs 19.1 download](https://github.com/CarlosJRay/LineageOs_sdm845/releases/tag/19.1)
- Published: 2022-10-17
- Release note: maintainer describes this as their first build and says to test at your own risk; listed supported device codenames are `starqltechn`, `star2qltechn`, and `crownqltechn`.
- Important: this is an unofficial historical release, not evidence of current security support or proof that every function works.

## Exact SM-G9650 / star2qltechn asset

**Download:** [lineage-19.1-20221025-UNOFFICIAL-star2qltechn.zip](https://github.com/CarlosJRay/LineageOs_sdm845/releases/download/19.1/lineage-19.1-20221025-UNOFFICIAL-star2qltechn.zip)

- Asset size reported by GitHub: 676,505,274 bytes (about 645 MiB)
- SHA-256: not published in the release asset metadata checked
- GitHub asset download count at audit time: 1,589
- Build date in filename: 2022-10-25

Do not use the `starqltechn` ZIP, which is for the S9 (G9600), on the S9+ (G9650). The exact S9+ asset is the `star2qltechn` ZIP.

## Why this release matters

This is a concrete published image for the target codename and should be treated as a historical comparison baseline, ahead of a loosely inferred source combination. If the user still has the exact ZIP used during previous tests, confirm whether its filename matches this asset; if it does, capture its local SHA-256 and build fingerprint after installation.

The release's GitHub metadata does not state a SHA-256 digest, and the release body warns that the build may contain errors. Do not label it fully stable solely because it is published or has download counts.

## Next reproducibility work

1. Recover the exact ROM ZIP used in the user's earlier test, including the filename and SHA-256.
2. If it matches the release above, use this image to confirm baseline device behavior and collect `ro.build.fingerprint`, kernel version, baseband, Bluetooth logs, and radio logs.
3. Reconstruct the source dependency set used by the release rather than combining similarly named but incompatible forks.
4. After the baseline manifest is reproducible, port forward to Android 16 and compare the results.

Do not commit ROM binaries, proprietary vendor dumps, personal device identifiers or access credentials into this source repository.
