# Android 16 compatibility gap analysis

Checked: 2026-10-13

## Target

- Device: Samsung Galaxy S9+ Snapdragon SM-G9650
- Device codename: `star2qltechn`
- Target platform: Android 16 / LineageOS `lineage-23.2`

## Findings

1. **The upstream Android 16 branch exists, but a matching G9650 device tree has not been found in the checked source set.** The relevant community Samsung SDM845 repositories located in this investigation have `lineage-20` or `lineage-20.0` branches. Exact-device source links are documented in [the LineageOS 20 reference source set](reference-source-set.md). Search results alone cannot prove no private or unindexed port exists, but no directly usable public `star2qltechn` / `lineage-23.2` tree has been verified here.
2. **The device tree is not the only porting task.** The shared tree includes older build variables, vendor integration, device properties and SELinux/init configuration. These must be reviewed against the Android 16 build system and interface rules.
3. **The kernel is an older Samsung SDM845 branch.** The LineageOS 20 source uses the S9+ dedicated kernel defconfig `lineage_star2qltechn_defconfig`. Android 16 compatibility, compiler compatibility, boot image format and required kernel interfaces need empirical validation; do not assume either that it works unchanged or that a total rewrite is automatically required.
4. **Vendor components are a major risk.** Current vendor tree revisions are from the Android 13-era source lineage and include proprietary blobs. Their original firmware source, version compatibility, VINTF declarations and redistribution rights need verification. Do not mirror these files into this project.
5. **The historical LineageOS 19.1 ZIP is a behavior reference, not the complete source manifest.** It can help reproduce Bluetooth/network observations but does not guarantee its corresponding source tree commits are exactly the latest commits now found on GitHub.

## Recommended technical sequence

### A. Freeze a reference combination

Use `local_manifests/lineage-20-reference.xml` with LineageOS branch `lineage-20`. First prove the pinned projects can be fetched together, the expected `lineage_star2qltechn-userdebug` lunch target resolves, and any build failures are captured. Do not label this set buildable until tested.

### B. Compare the Android 16 upstream interfaces

Use LineageOS `lineage-23.2` as the platform base. Review the following surfaces in a fixed order:

1. Product makefiles / board config / lunch target.
2. Kernel build rules, toolchain, defconfig, modules, image format and boot header.
3. VINTF manifest and vendor compatibility matrix.
4. SELinux policy and init scripts.
5. Proprietary blobs and their dependencies (graphics, media, audio, sensors, camera, radio/IMS).
6. Recovery/install package assumptions, fstab, encryption and AVB.
7. Runtime validation, beginning with boot, RIL/network registration, Bluetooth, Wi-Fi and audio.

### C. Keep the Android 16 tree separate

Once each dependency is verified, fork or patch only the required sources and pin them in an Android 16 manifest. Do not commit a manifest that simply substitutes `lineage-23.2` for `lineage-20` until its projects and required companion branches have been confirmed.

## Evidence and stop conditions

For each compatibility change, record source SHA, failure log, patch, rebuilt component and test outcome. Stop before flashing if:
- the product assert does not explicitly target `star2qltechn` / SM-G9650;
- the exact source/target image has not been identified;
- backup and recovery route are not ready;
- vendor blobs or firmware are mismatched;
- a critical radio, charging or boot failure remains unexplained.

## Current conclusion

The source evidence supports a staged port using the pinned LineageOS 20 code as an integration checkpoint, then moving to `lineage-23.2`. It does **not** yet establish a ready-to-build Android 16 manifest or a tested Android 16 kernel/vendor combination.
