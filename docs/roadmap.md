# Project roadmap: G9650 Android 16

Last reviewed: 2026-10-13

## Goal

Produce a reproducible LineageOS-based Android 16 ROM for Samsung Galaxy S9+ Snapdragon SM-G9650 (`star2qltechn`) and validate it on actual hardware. A successful compile or first boot alone does not mean daily-use readiness.

Target LineageOS branch: `lineage-23.2` (Android 16). A matching public G9650-specific `lineage-23.2` device tree has not been verified in this audit, so device integration remains porting work.

## Current state

- [x] Dedicated public repository created.
- [x] Initial project documentation and GitHub Actions config-validation workflow added.
- [x] Identified the published community LineageOS 19.1 asset for `star2qltechn`.
- [x] Pinned a candidate LineageOS 20 reference source set for an integration checkpoint.
- [x] Recorded the stale `lineage.dependencies` mismatch and the deliberate common-tree override in [reference-source-set.md](reference-source-set.md).
- [ ] Verify the reference manifest by syncing a LineageOS 20 tree.
- [ ] Confirm the expected lunch target resolves and the ROM builds, or document the first genuine blocker.
- [ ] Reconcile vendor blobs, firmware provenance, and redistribution constraints.
- [ ] Create a dedicated Android 16 port branch and manifest.
- [ ] Resolve Android 16 build-system, kernel, vendor/HAL, VINTF, SELinux, init, image-packaging and compatibility errors.
- [ ] Produce a flashable test image and record checksums.
- [ ] Test on SM-G9650 and publish a feature-by-feature report.

## Milestones and acceptance gates

### M0 — Project structure and safety

**Done when:** documentation, reference sources, manifest files and validation workflow are versioned; no full Android tree, ROM image, private signing key, credentials or unapproved proprietary binaries are committed here.

### M1 — Reference source lock (current milestone)

Use the pinned LineageOS 20 sources in [lineage-20-reference.xml](../local_manifests/lineage-20-reference.xml) as an integration checkpoint. The XML explicitly overrides a stale common-tree dependency from the exact-device tree; this must be verified by the actual checkout and build tools. The 2022 release image is a behavior reference, not proof that these 2023 source commits recreate the release build.

**Acceptance gate:**
- All pinned projects clone/fetch successfully.
- `repo sync` completes without missing projects.
- The final checked-out common tree is the intended `sdm845-samsung-dev` revision, not an auto-fetched older fork.
- `lineage_star2qltechn-userdebug` is recognized.
- All direct/transitive dependencies are documented.
- A build attempt finishes or its first actionable failure is captured.

### M2 — Android 16 port branch

Start from official LineageOS `lineage-23.2` and port/update the device-specific tree, shared tree, kernel integration and vendor configuration. Keep the working reference set separate; never silently mutate the only baseline.

**Acceptance gate:**
- Target product parses and builds far enough to produce a defined image.
- No unresolved duplicate project paths or invalid manifest refs.
- Each compatibility change has a patch, build result and rationale.

### M3 — Boot and radio bring-up

Test first boot, repeat boot, recovery access, SIM registration, calls, SMS, mobile data, Wi-Fi and Bluetooth. Capture exact build fingerprint, bootloader/baseband information, logs and failure conditions.

### M4 — Daily-use validation

Test audio/microphone, cameras, sensors, fingerprint, GNSS, NFC if supported, charging, battery reporting, sleep/wake, encryption, thermal behavior and stability over repeated reboots and ordinary daily use. Document failures honestly.

## Build infrastructure

GitHub Actions can run config checks and coordinate work, but a full Android source build needs much more disk and memory than a standard hosted runner provides. Use a supported larger runner or a secured cloud self-hosted runner. Plan for at least 64 GB RAM and ample high-speed storage; roughly 1 TB or more of free workspace is a planning target, not a measured project requirement. Confirm the precise requirements and cost before provisioning.

Never expose a self-hosted runner to untrusted pull requests. Keep signing credentials out of logs and do not enable release signing until the source and build pipeline are trusted.

## Current risks / blockers

1. The exact-device tree's dependency file points to an older common tree whose filter omits the `techn` codename; the reference manifest overrides it. This needs checkout/build verification.
2. The historical source sets use several forks; similar names do not guarantee compatible branches.
3. The S9+ device-tree commit inspected is dated after the historical ROM release asset upload, so it may not recreate the exact release build.
4. Samsung vendor trees may contain proprietary files. Do not mirror them into this public repository unless redistribution rights are clear.
5. Android 16 has no verified G9650-specific target source set in this project yet. Kernel/vendor compatibility must be demonstrated, not assumed.
6. Hardware testing requires the physical SM-G9650; cloud CI cannot prove radio or Bluetooth behavior.

## Working convention

One milestone at a time. Every proposed change should include exact source commit(s), failure or reason being addressed, expected result, build/check result and real-device test result when applicable. Keep experiments isolated from the reference baseline.
