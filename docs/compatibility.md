# Source compatibility audit: G9650 / SDM845

## Target

- Device: Samsung Galaxy S9+ Snapdragon, SM-G9650
- Codename: `star2qltechn`
- SoC family: Qualcomm SDM845
- Goal: LineageOS-based Android 16, with device features validated on the actual handset

## Rules for source selection

The existence of a branch or repository is not proof that it builds for this exact SKU. Before choosing sources, record the repository URL, branch, latest commit SHA, target Android version, target device(s), and any known working features.

Audit these components together:

| Component | What to verify |
|---|---|
| Device tree | Correct codename/SKU, product makefiles, init scripts, overlays, partition/image definitions, sepolicy |
| SDM845 common tree | Device-family configuration and shared dependencies match chosen device-tree revision |
| Kernel | Correct Samsung/SDM845 device support, defconfig, modules, headers, boot image format and Android interface requirements |
| Vendor blobs | Correct source firmware and matching library/service dependencies; do not redistribute proprietary blobs without authorization |
| Hardware interfaces | Android 16 compatibility of audio, camera, graphics, media, sensors, lights, biometrics and other HAL/services |
| Radio stack | Samsung RIL, vendor radio services, modem firmware/baseband and carrier/network behavior |
| Recovery / flashing | Partition map, dynamic/static partitions as applicable, AVB/dm-verity state, encryption, package compatibility |
| Build manifests | Every project URL, branch/tag and revision resolves; revisions are mutually compatible |

## Suggested comparison baseline

Use the exact LineageOS 19.1 source revision that was previously used by the device as the initial reference, if it can be recovered. Community LineageOS 20/Android 13 device-tree and common-tree revisions can be examined as an intermediate comparison point; they must not be treated as Android 16-ready without a per-component audit.

For every reused component, classify it as one of:

- **Reuse candidate:** source revision builds and interfaces are compatible with the selected target.
- **Port required:** useful code exists but needs Android 16 updates.
- **Replace required:** critical interfaces, build files or kernel/vendor assumptions are incompatible.
- **Unknown:** insufficient evidence until build logs or device testing exist.

## Compatibility audit output

Record the following for each source:
- Repository and branch
- Commit SHA/date
- Last known Android version supported
- Required companion repositories
- Build status and exact failure
- Hardware test evidence
- Redistribution/license notes

## Important caution

Android version upgrades affect more than the device tree. Kernel, vendor blobs, VINTF compatibility, sepolicy, init, radio services, graphics/media stack and image packaging must be considered as one system. Do not mix arbitrary branches solely because they use the same SoC.