# G9650 LineageOS 19.1 baseline — revised audit

Audit date: 2026-10-13

## Confirmed exact device tree

The relevant repository is [CarlosJRay/Android_device_samsung_star2qltechn](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1), branch `lineage-19.1`, commit `06e450924ba754d141b1b8df04259931f175c30d`.

Inspected files confirm:
- Product makefile: `lineage_star2qltechn.mk`
- `PRODUCT_DEVICE := star2qltechn`
- `PRODUCT_MODEL := SM-G9650`
- BoardConfig kernel config: `star2qlte_chn_open_defconfig`
- Device tree `lineage.dependencies` references `matebuteler/android_device_samsung_sdm845-common`.

This is the correct device-specific reference tree to investigate further for G9650. It does not by itself prove this repo, in isolation, reproduces the exact ROM the user tested or that every hardware feature works.

## Repositories checked

| Layer | Repository/ref | Evidence | Status |
|---|---|---|---|
| Exact S9+ device tree | [CarlosJRay/Android_device_samsung_star2qltechn](https://github.com/CarlosJRay/Android_device_samsung_star2qltechn/tree/lineage-19.1), `lineage-19.1` | `06e450924ba754d141b1b8df04259931f175c30d`; product says SM-G9650 / star2qltechn | **Best initial device-tree candidate** |
| S9 device tree | [CarlosJRay/Android_device_samsung_starqltechn](https://github.com/CarlosJRay/Android_device_samsung_starqltechn/tree/lineage-19.1), `lineage-19.1` | `b04346a4590c0eee9bdc5d21dad32648eab660e9`; product says SM-G9600 / starqltechn | Reference only; not target |
| Shared device tree A | [CarlosJRay/android_device_samsung_sdm845-common](https://github.com/CarlosJRay/android_device_samsung_sdm845-common/tree/lineage-19.1), `lineage-19.1` | `e5e084a0b67fae28804c54f702a0522be038c721`; makefile filter includes star2qltechn; dependency file names `ruizlenato/android_kernel_samsung_sdm845` and DotOS vendor branch `dot11` | Related alternative, but dependency graph differs from exact device tree |
| Shared device tree B | [matebuteler/android_device_samsung_sdm845-common](https://github.com/matebuteler/android_device_samsung_sdm845-common) | Exact S9+ tree names it in `lineage.dependencies`, but this repository's only listed branch is `lineage-18.1` at `22d51764fdaf952125ec7cfde4b79b06fe1a3539`; there is no `lineage-19.1` branch | **Dependency mismatch requiring resolution** |
| Vendor candidate supplied | [CarlosJRay/android_vendor_samsung_sdm845-common](https://github.com/CarlosJRay/android_vendor_samsung_sdm845-common) `main` | `55e1111e92f7aaa748c4f192a7da31a31ede273c`; current branch not proven to be the exact-device 19.1 vendor | Not pinned yet |
| Vendor listed by shared tree A | [DotOS-Devices/proprietary_vendor_samsung_sdm845-common](https://github.com/DotOS-Devices/proprietary_vendor_samsung_sdm845-common), `dot11` | Explicitly named by shared tree A; proprietary components have redistribution constraints | Candidate only |
| Kernel candidate supplied | [Nyoutaaa/android_kernel_samsung_sdm845](https://github.com/Nyoutaaa/android_kernel_samsung_sdm845/tree/thirteen), `thirteen` | `a44036cfc48932ee1de3f55c021265bb141fb7f6`; branch label indicates Android 13-era line | Candidate, not confirmed as 19.1 kernel |
| Kernel listed by shared tree A | `ruizlenato/android_kernel_samsung_sdm845` | Exact dependency name in common-tree A, but repository URL could not be resolved through the current audit lookup | Need resolve exact upstream/fork and revision |

## Why the sources must not simply be combined

The exact S9+ device tree says to fetch `matebuteler/android_device_samsung_sdm845-common`, but that repo currently exposes only a `lineage-18.1` branch. Separately, CarlosJRay's common tree points to a different kernel repository and DotOS vendor branch. These differences need to be reconciled before generating a build manifest.

Do not treat the shared-tree reference as interchangeable just because it targets the same SoC. Resolve the exact dependency revision or determine which fork/branch the original working build used.

## Source dependency map observed

```text
CarlosJRay/Android_device_samsung_star2qltechn (lineage-19.1)
  └── matebuteler/android_device_samsung_sdm845-common
      └── currently only lineage-18.1 listed; investigate matching 19.1 fork/revision

Separate alternative:
CarlosJRay/android_device_samsung_sdm845-common (lineage-19.1)
  ├── ruizlenato/android_kernel_samsung_sdm845 (URL/revision unresolved)
  └── DotOS-Devices/proprietary_vendor_samsung_sdm845-common (dot11)
```

## Next steps

1. Find which SDM845 common-tree fork/commit was actually paired with the exact S9+ tree when the LineageOS 19.1 build was produced.
2. Find the exact kernel source/revision from that dependency graph; inspect whether `star2qlte_chn_open_defconfig` exists in it.
3. Identify the matching vendor blobs from the ROM's source firmware/version. Do not copy proprietary blobs into this public project unless redistribution is clearly allowed.
4. Commit a `local_manifests/*.xml` only after all repository branches and dependencies resolve consistently.
5. Build the historical baseline first, then use it as a reference for Android 16 porting.

The S9 (SM-G9600) tree is retained solely as a comparison source and must not be flashed on the S9+.
