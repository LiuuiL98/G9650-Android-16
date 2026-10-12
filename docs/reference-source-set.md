# Pinned LineageOS 20 reference source set

Checked: 2026-10-13

This is a **provisional Android 13 / LineageOS 20 integration checkpoint**, not the Android 16 manifest and not yet a verified build. The reference XML is [`local_manifests/lineage-20-reference.xml`](../local_manifests/lineage-20-reference.xml).

## Pinned projects

| Project path | Remote repository | Pinned commit | Purpose |
|---|---|---|---|
| `device/samsung/star2qltechn` | [sdm845-samsung-dev/android_device_samsung_star2qltechn](https://github.com/sdm845-samsung-dev/android_device_samsung_star2qltechn/tree/lineage-20) | `ba5bc312b8fd638093b7ac5a0604986342ed29d7` | Exact target device; product says SM-G9650 and selects `lineage_star2qltechn_defconfig` |
| `device/samsung/sdm845-common` | [sdm845-samsung-dev/android_device_samsung_sdm845-common](https://github.com/sdm845-samsung-dev/android_device_samsung_sdm845-common/tree/lineage-20) | `c13b15e6392b4d15ae18d8d8ea64560f0ee02f88` | Common SDM845 tree whose build filter explicitly includes `star2qltechn` |
| `hardware/samsung` | [LineageOS/android_hardware_samsung](https://github.com/LineageOS/android_hardware_samsung/tree/lineage-20) | `65fcb7bb55a35d6751bbe289fcba894fce39becf` | LineageOS Samsung hardware integration |
| `kernel/samsung/sdm845` | [sdm845-samsung-dev/android_kernel_samsung_sdm845](https://github.com/sdm845-samsung-dev/android_kernel_samsung_sdm845/tree/lineage-20) | `aa7f013da287f80642982f582f4035fbabd5fe89` | SDM845 kernel; the pinned commit adds `lineage_star2qltechn_defconfig` |
| `vendor/samsung/sdm845-common` | [sdm845-samsung-dev/android_vendor_samsung_sdm845-common](https://github.com/sdm845-samsung-dev/android_vendor_samsung_sdm845-common/tree/lineage-20) | `bef4f36124cebc85c12c8cab6e0279895c82f53e` | Vendor components, with makefile filter that includes the `techn` codenames |

## Important dependency correction

The exact-device tree's own [`lineage.dependencies`](https://github.com/sdm845-samsung-dev/android_device_samsung_star2qltechn/blob/lineage-20/lineage.dependencies) points to `sdm845-samsung/android_device_samsung_sdm845-common`, an older fork.

That older common tree's [`Android.mk`](https://github.com/sdm845-samsung/android_device_samsung_sdm845-common/blob/lineage-20/Android.mk) filters `crownqlte star2qlte starqlte` without the `techn` aliases, so it does not explicitly match the target product name `star2qltechn`.

The pinned manifest therefore **intentionally overrides that stale dependency path** with [`sdm845-samsung-dev/android_device_samsung_sdm845-common`](https://github.com/sdm845-samsung-dev/android_device_samsung_sdm845-common/blob/lineage-20/Android.mk), whose filter includes `crownqltechn starqltechn star2qltechn`. The kernel and vendor entries are pinned to the same `sdm845-samsung-dev` source family and include the corresponding `star2qltechn` configuration/filters.

This is an informed integration choice, **not yet a proven working build**. The next step must verify `repo sync`, `lunch lineage_star2qltechn-userdebug`, and a real build attempt. If any project path is re-fetched or overwritten by dependency tooling, inspect the final checked-out remotes and commits before proceeding.

## Why this set is useful

- The exact-device product makefile identifies `star2qltechn` / `SM-G9650`.
- The chosen shared tree explicitly accepts the S9+ `techn` codename.
- The kernel has an explicit S9+ LineageOS defconfig.
- Revisions are pinned to full Git commit IDs for reproducibility.

## What is not yet proven

- The projects have not yet been cloned together or built from this project.
- The vendor files may be proprietary. This project records a reference only and does not mirror the vendor tree contents.
- This set targets LineageOS 20 / Android 13 and is not an Android 16 target manifest.
- The 2022 LineageOS 19.1 release does not necessarily correspond to these 2023 commits.
- Hardware functionality must be tested on an actual SM-G9650.

## Reference checkout procedure (not for Android 16)

Use a disposable workspace with enough free disk space. The base manifest and local projects must all target the LineageOS 20 branch:

```bash
repo init -u https://github.com/LineageOS/android.git -b lineage-20.0 --git-lfs --no-clone-bundle
mkdir -p .repo/local_manifests
cp /path/to/lineage-20-reference.xml .repo/local_manifests/
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune -j8
source build/envsetup.sh
lunch lineage_star2qltechn-userdebug
```

This is a reference validation flow only. It does not imply that source sync or build has succeeded yet. Do not flash any image until the source, checksum, target assertion and recovery path have been verified.
