# G9650 LineageOS 19.1 baseline — revised audit

Audit date: 2026-10-13

## Critical correction: an exact G9650 device tree exists

The user initially supplied the S9 device-tree repository:
- [CarlosJRay/Android_device_samsung_starqltechn, lineage-19.1](https://github.com/CarlosJRay/Android_device_samsung_starqltechn/tree/lineage-19.1) — its product file identifies `SM-G9600`, the Galaxy S9, not the target S9+.

A separate exact device-specific repository has now been located:
- [CarlosJRay/Android_device_samsung_star2qltechn, lineage-19.1](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1)
- Branch commit: `06e450924ba754d141b1b8df04259931f175c30d`
- `PRODUCT_DEVICE := star2qltechn`
- `PRODUCT_MODEL := SM-G9650`
- Product file: `lineage_star2qltechn.mk`
- Kernel config: `star2qlte_chn_open_defconfig`
- Device-specific `lineage.dependencies` points to `matebuteler/android_device_samsung_sdm845-common`.

**This exact-device branch is the better starting point for recreating the LineageOS 19.1 baseline.** It still needs a full manifest/dependency and actual build verification; repository presence alone does not prove a successful ROM or fully working hardware.

## Repositories and branches observed

| Layer | Repository/ref | Revision or observed details | Status |
|---|---|---|---|
| Exact S9+ device tree | [CarlosJRay/Android_device_samsung_star2qltechn](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1) `lineage-19.1` | `06e450924ba754d141b1b8df04259931f175c30d`; product = SM-G9650 / star2qltechn | **Best initial device-tree candidate** |
| S9 device tree (not target) | [CarlosJRay/Android_device_samsung_starqltechn](https://github.com/CarlosJRay/Android_device_samsung_starqltechn/tree/lineage-19.1) `lineage-19.1` | `b04346a4590c0eee9bdc5d21dad32648eab660e9`; product = SM-G9600 / starqltechn | Use only to compare shared S9/S9+ deltas |
| Shared device tree candidate A | [CarlosJRay/android_device_samsung_sdm845-common](https://github.com/CarlosJRay/android_device_samsung_sdm845-common/tree/lineage-19.1) `lineage-19.1` | `e5e084a0b67fae28804c54f702a0522be038c721`; accepts star2qltechn in makefile filter; dependencies use ruizlenato kernel and DotOS vendor `dot11` | Historical common-tree candidate, dependency source differs |
| Shared device tree candidate B | [matebuteler/android_device_samsung_sdm845-common](https://github.com/matebuteler/android_device_samsung_sdm845-common) | Explicit dependency of the exact S9+ device tree; exact branch/commit must be checked before locking manifest | **Next item to verify** |
| Vendor candidate supplied by user | [CarlosJRay/android_vendor_samsung_sdm845-common](https://github.com/CarlosJRay/android_vendor_samsung_sdm845-common) `main` | `55e1111e92f7aaa748c4f192a7da31a31ede273c`; not clearly shown as the exact S9+ baseline in the inspected build filter | Do not combine blindly |
| Vendor required by common-tree dependency | [DotOS-Devices/proprietary_vendor_samsung_sdm845-common](https://github.com/DotOS-Devices/proprietary_vendor_samsung_sdm845-common), branch `dot11` | Explicit dependency in CarlosJRay common tree; proprietary files may have redistribution constraints | Historical dependency candidate |
| Kernel candidate supplied by user | [Nyoutaaa/android_kernel_samsung_sdm845](https://github.com/Nyoutaaa/android_kernel_samsung_sdm845/tree/thirteen) `thirteen` | `a44036cfc48932ee1de3f55c021265bb141fb7f6`; Android 13-era branch label | Alternative candidate; not established as the 19.1 kernel |
| Kernel required by common-tree dependency | `ruizlenato/android_kernel_samsung_sdm845` | Named by `CarlosJRay/android_device_samsung_sdm845-common/lineage-19.1/lineage.dependencies`; repository lookup was not resolvable in this audit environment | Need recover/resolve exact source URL and revision |

## Dependency graph from the files inspected

```text
device/samsung/star2qltechn
  └── matebuteler/android_device_samsung_sdm845-common
      └── must inspect its own dependencies before selecting kernel/vendor

Separate CarlosJRay common tree:
device/samsung/sdm845-common (CarlosJRay, lineage-19.1)
  ├── ruizlenato/android_kernel_samsung_sdm845
  └── DotOS-Devices/proprietary_vendor_samsung_sdm845-common (dot11)
```

Do not assume the two shared device-tree candidates are interchangeable. Follow the dependency graph of the exact-device tree first, then compare against alternatives. Keep every selected repository's commit SHA in the project manifest notes.

## Recommended next action

Inspect [matebuteler/android_device_samsung_sdm845-common](https://github.com/matebuteler/android_device_samsung_sdm845-common), its branch and dependency file; then locate the exact kernel revision and vendor branch used with it. Only after that create the first `local_manifests/*.xml` for a reproducible 19.1 baseline. A separate Android 16 work branch can then be compared against it.
