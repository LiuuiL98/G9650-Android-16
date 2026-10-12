# G9650 LineageOS 19.1 baseline — revised audit

Audit date: 2026-10-13

## Confirmed exact device tree

[**CarlosJRay/Android_device_samsung_star2qltechn**](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1), branch `lineage-19.1`, commit `06e450924ba754d141b1b8df04259931f175c30d), is the exact device-specific source candidate.

Inspected files confirm:
- README explicitly describes the Samsung Galaxy S9+ / `star2qltechn`.
- Product makefile declares `PRODUCT_DEVICE := star2qltechn` and `PRODUCT_MODEL := SM-G9650`.
- BoardConfig declares `TARGET_OTA_ASSERT_DEVICE := star2qltechn`.
- Kernel config is `star2qlte_chn_open_defconfig`.
- Device `lineage.dependencies` points to `matebuteler/android_device_samsung_sdm845-common`.

This is the best starting point for reproducing the historical LineageOS 19.1 baseline, subject to dependency validation and actual build/test confirmation.

## Repositories and revisions observed

| Layer | Repository/ref | Evidence | Status |
|---|---|---|---|
| Exact S9+ device tree | [CarlosJRay/Android_device_samsung_star2qltechn](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1), `lineage-19.1` | `06e450924ba754d141b1b8df04259931f175c30d`; product `star2qltechn` / SM-G9650 | **First-choice device tree** |
| S9 (not S9+) tree | [CarlosJRay/Android_device_samsung_starqltechn](https://github.com/CarlosJRay/Android_device_samsung_starqltechn/tree/lineage-19.1), `lineage-19.1` | `b04346a4590c0eee9bdc5d21dad32648eab660e9`; product `starqltechn` / SM-G9600 | Comparison only |
| Shared tree A | [CarlosJRay/android_device_samsung_sdm845-common](https://github.com/CarlosJRay/android_device_samsung_sdm845-common/tree/lineage-19.1), `lineage-19.1` | `e5e084a0b67fae28804c54f702a0522be038c721`; dependencies name a kernel and DotOS vendor `dot11` branch | Related but not yet proven the exact-device dependency |
| Shared tree B | [matebuteler/android_device_samsung_sdm845-common](https://github.com/matebuteler/android_device_samsung_sdm845-common) | Exact device tree's declared dependency; its only listed branch is `lineage-18.1`, commit `22d51764fdaf952125ec7cfde4b79b06fe1a3539` | **Mismatch: requires further resolution** |
| Vendor tree supplied | [CarlosJRay/android_vendor_samsung_sdm845-common](https://github.com/CarlosJRay/android_vendor_samsung_sdm845-common) `main` | `55e1111e92f7aaa748c4f192a7da31a31ede273c`; commit message says blobs updated to `QP1A.190711.020` (Android 10-era firmware) | Not proven to match the 19.1 baseline |
| Vendor named by shared tree A | [DotOS-Devices/proprietary_vendor_samsung_sdm845-common](https://github.com/DotOS-Devices/proprietary_vendor_samsung_sdm845-common), branch `dot11` | Explicit in shared tree A dependency file | Candidate only; proprietary redistribution must be reviewed |
| Kernel candidate supplied | [Nyoutaaa/android_kernel_samsung_sdm845](https://github.com/Nyoutaaa/android_kernel_samsung_sdm845/tree/thirteen), branch `thirteen` | `a44036cfc48932ee1de3f55c021265bb141fb7f6`; this revision modifies the `star2qlte_chn_open_defconfig` stack-protector setting | Good comparison candidate, not confirmed as the 19.1 kernel |
| Kernel named by shared tree A | `ruizlenato/android_kernel_samsung_sdm845` | Name found in dependency file; repository could not be resolved via this audit lookup | Exact URL/revision unresolved |

## Dependency graph observed

```text
CarlosJRay/android_device_samsung_star2qltechn (lineage-19.1)
  └── matebuteler/android_device_samsung_sdm845-common
      └── only lineage-18.1 branch found — investigate original fork/revision

Separate alternative:
CarlosJRay/android_device_samsung_sdm845-common (lineage-19.1)
  ├── ruizlenato/android_kernel_samsung_sdm845 (unresolved)
  └── DotOS-Devices/proprietary_vendor_samsung_sdm845-common (dot11)
```

The supplied S9+ device tree explicitly says it is a bring-up of Nyoutaaa's tree from dotOS to LineageOS. The Nyoutaaa kernel has a named S9+ China defconfig, which makes it relevant to inspect, but that alone does not prove it was the exact paired kernel for this LineageOS 19.1 commit.

Do not combine the above dependency lines arbitrarily. Either resolve the declared common-tree dependency to the matching 19.1 revision or identify the exact tree/manifest from the original build.

## Why the supplied S9 tree still matters

The S9 (SM-G9600 / `starqltechn`) tree is related and useful for shared-device comparisons, but the S9+ product tree must remain selected for SM-G9650. Do not change product assertions or flash an S9-targeted image on the S9+.

## Android 16 strategy

1. Reconstruct the reproducible 19.1 source bundle first and record all SHAs.
2. Use that baseline and relevant Android 13-era forks to identify which kernel/device/vendor interfaces can be carried forward.
3. Port and resolve Android 16 build, VINTF, SELinux, init, media/graphics, radio and other HAL requirements incrementally.
4. Commit a `local_manifests/*.xml` only after the chosen device/common/kernel/vendor revisions all exist and are compatible.
5. Build and test on an actual SM-G9650; do not equate successful compilation or first boot with daily-use stability.

No full Android source tree or proprietary vendor blobs have been copied into this project.
