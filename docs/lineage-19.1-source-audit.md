# LineageOS 19.1 source baseline audit (G9650 / star2qltechn)

Audit date: 2026-10-13

This document records the community repositories supplied for the known Android 12L / LineageOS 19.1 port. It is an audit snapshot, not proof that these repositories build together or that all hardware features work on every G9650.

## Repository inventory

| Component | Repository / ref | Audit findings | Current classification |
|---|---|---|---|
| Device-specific tree | [CarlosJRay/Android_device_samsung_starqltechn](https://github.com/CarlosJRay/Android_device_samsung_starqltechn/tree/lineage-19.1), branch `lineage-19.1`, commit `b04346a4590c0eee9bdc5d21dad32648eab660e9` | Product makefile identifies PRODUCT_DEVICE as `starqltechn`, model `SM-G9600`; BoardConfig selects `starqlte_chn_open_defconfig`. | **Important identity mismatch to verify** |
| SDM845 common device tree | [CarlosJRay/android_device_samsung_sdm845-common](https://github.com/CarlosJRay/android_device_samsung_sdm845-common/tree/lineage-19.1), branch `lineage-19.1`, commit `e5e084a0b67fae28804c54f702a0522be038c721` | Explicit Android 12L / LineageOS 19.1 branch. Its `Android.mk` target filter includes `crownqltechn starqltechn star2qltechn`. `BoardConfigCommon.mk` targets SDM845 and Adreno 630, uses 64-bit primary and 32-bit secondary architecture settings, and legacy kernel/image values. | **Reference baseline only** |
| Vendor common tree | [CarlosJRay/android_vendor_samsung_sdm845-common](https://github.com/CarlosJRay/android_vendor_samsung_sdm845-common), branch `main`, commit `55e1111e92f7aaa748c4f192a7da31a31ede273c` | Repo contains proprietary Samsung vendor components and generated build definitions. In the inspected `Android.mk`, a filter mentions `star2qlte starqlte`, not the `star2qltechn` codename exactly. | **Do not assume this branch matches the device tree** |
| Kernel candidate | [Nyoutaaa/android_kernel_samsung_sdm845](https://github.com/Nyoutaaa/android_kernel_samsung_sdm845), branch `thirteen`, commit `a44036cfc48932ee1de3f55c021265bb141fb7f6` | Branch name suggests Android 13-era use, but this is not a confirmed match for the LineageOS 19.1 tree. The common tree's `lineage.dependencies` instead points to `ruizlenato/android_kernel_samsung_sdm845`. | **Alternative candidate; compatibility unproven** |

## Critical device-name distinction

The supplied device tree is named `starqltechn`, and its product makefile sets model `SM-G9600`. Your phone is SM-G9650 / `star2qltechn`.

In Samsung Galaxy naming, `starqltechn` is associated with the S9 Snapdragon China/HK family and `star2qltechn` with the S9+ Snapdragon China/HK family. They share SDM845 components, but the phone-specific product configuration, display/panel, camera, sensors, partition layout, firmware, device assert, and kernel configuration must not be blindly reused. The common tree's filter includes `star2qltechn`, but that alone does not mean the supplied phone-specific `starqltechn` tree can build the S9+.

**Do not point `PRODUCT_DEVICE` at the G9600 tree and attempt to flash it on the G9650.**

## Dependency conflict to resolve before a manifest is committed

The inspected `lineage.dependencies` in the supplied star device tree points to:

- `dotOS-Devices/android_device_samsung_sdm845-common`

The inspected common tree's `lineage.dependencies` points to:

- `ruizlenato/android_kernel_samsung_sdm845`
- `DotOS-Devices/proprietary_vendor_samsung_sdm845-common`, branch `dot11`

This differs from the supplied CarlosJRay common tree, vendor tree and Nyoutaaa kernel. Therefore, the four links are useful evidence of related community work, but must not simply be combined into a manifest without checking dependency revisions and target compatibility.

## Immediate follow-up investigation

1. Find a **G9650 / star2qltechn-specific device tree** with an explicit 19.1 or later branch, preferably one whose product files identify `SM-G9650` and `PRODUCT_DEVICE := star2qltechn`.
2. Inspect the actual full common-tree dependency manifest and kernel defconfig names.
3. Compare the supplied vendor tree's build filters, device-specific blobs and licensing/redistribution implications before relying on it.
4. Check which kernel repository and exact commit were used by the actual working ROM. A different branch named `thirteen` should not replace the known kernel without a source/build comparison.
5. Only after dependencies are pinned, create a `local_manifests` XML and run `repo sync` in a build environment.
6. Retain the known LineageOS 19.1 setup as a reference point for triaging radio and Bluetooth issues. Do not assume those reported issues are caused by any one of these repos until logs are available.

## Current conclusion

These links establish a useful historical SDM845 / LineageOS 19.1 reference. However, the named device tree is for `starqltechn` / SM-G9600, whereas the project target is `star2qltechn` / SM-G9650. It can inform the shared-platform analysis, but the correct S9+ device tree is still the first missing link. The vendor and kernel sources also need matching-revision verification.

No proprietary vendor images or full source trees have been copied into this repository.
