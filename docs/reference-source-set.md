# Pinned LineageOS 20 reference source set

Checked: 2026-10-13

This set is intentionally an Android 13 / LineageOS 20 **integration checkpoint**, not the final Android 16 manifest. The reference XML is [`local_manifests/lineage-20-reference.xml`](../local_manifests/lineage-20-reference.xml).

| Project path | Remote repository | Pinned commit | Purpose |
|---|---|---|---|
| `device/samsung/star2qltechn` | [sdm845-samsung-dev/android_device_samsung_star2qltechn](https://github.com/sdm845-samsung-dev/android_device_samsung_star2qltechn/tree/lineage-20) | `ba5bc312b8fd638093b7ac5a0604986342ed29d7` | Exact target device; product says SM-G9650 and uses `lineage_star2qltechn_defconfig` |
| `device/samsung/sdm845-common` | [sdm845-samsung-dev/android_device_samsung_sdm845-common](https://github.com/sdm845-samsung-dev/android_device_samsung_sdm845-common/tree/lineage-20) | `c13b15e6392b4d15ae18d8d8ea64560f0ee02f88` | Common SDM845 device support; dependency file includes Samsung hardware and kernel |
| `hardware/samsung` | [LineageOS/android_hardware_samsung](https://github.com/LineageOS/android_hardware_samsung/tree/lineage-20) | `65fcb7bb55a35d6751bbe289fcba894fce39becf` | LineageOS Samsung hardware integration |
| `kernel/samsung/sdm845` | [sdm845-samsung-dev/android_kernel_samsung_sdm845](https://github.com/sdm845-samsung-dev/android_kernel_samsung_sdm845/tree/lineage-20) | `aa7f013da287f80642982f582f4035fbabd5fe89` | Matching SDM845 kernel; commit adds `lineage_star2qltechn_defconfig` |
| `vendor/samsung/sdm845-common` | [sdm845-samsung-dev/android_vendor_samsung_sdm845-common](https://github.com/sdm845-samsung-dev/android_vendor_samsung_sdm845-common/tree/lineage-20) | `bef4f36124cebc85c12c8cab6e0279895c82f53e` | Samsung vendor components used by this historical source set |

## Why this set is useful

Unlike the previously inspected fragments, these device/common/kernel/vendor projects are grouped by the same `sdm845-samsung-dev` source lineage and branch family. The exact-device tree references this organization's shared device tree, and that shared tree references LineageOS Samsung hardware plus the organization's kernel. The kernel contains the dedicated S9+ LineageOS defconfig.

## What is not yet proven

- This pinned set has not yet been synced or built from this project.
- The vendor files may be proprietary; this project only records a repository reference and does not reproduce its contents.
- This set is based on LineageOS 20 / Android 13 and cannot be used as an Android 16 target manifest.
- The 2022 LineageOS 19.1 release does not necessarily correspond to these 2023 commits.
- Hardware functionality must be tested on an actual SM-G9650.

## Reference checkout procedure (not for Android 16)

Use a disposable checkout/workspace with enough disk space. The root manifest and these local projects must target the same LineageOS branch:

```bash
repo init -u https://github.com/LineageOS/android.git -b lineage-20 --git-lfs --no-clone-bundle
mkdir -p .repo/local_manifests
cp /path/to/lineage-20-reference.xml .repo/local_manifests/
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune -j8
source build/envsetup.sh
lunch lineage_star2qltechn-userdebug
```

This is a **reference validation** workflow only. It does not imply that a ROM build has already succeeded. Do not flash an image until its source, checksum, target assertion and recovery path have been verified.
