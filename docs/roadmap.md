# Project roadmap: G9650 Android 16

Last reviewed: 2026-10-13

## Goal

Produce a reproducible LineageOS-based Android 16 ROM for Samsung Galaxy S9+ Snapdragon SM-G9650 (`star2qltechn`) and validate it on actual hardware. A successful compile or first boot alone does not mean daily-use readiness.

Target LineageOS branch: `lineage-23.2` (Android 16). There is no verified G9650-specific `lineage-23.2` device tree identified in this audit yet, so device integration remains porting work.

## Current state

- [x] Dedicated public repository created.
- [x] Initial project documentation and config-validation Actions workflow added.
- [x] Identified the published community LineageOS 19.1 asset for `star2qltechn`.
- [x] Identified a coherent LineageOS 20 reference source set under `sdm845-samsung-dev` with an exact-device tree and corresponding kernel configuration.
- [ ] Verify the reference manifest by syncing a LineageOS 20 tree.
- [ ] Confirm a reference target can be resolved and the ROM builds.
- [ ] Reconcile vendor blobs, firmware provenance, and redistribution constraints.
- [ ] Create a dedicated Android 16 port branch and manifest.
- [ ] Resolve Android 16 build-system, kernel, vendor/HAL, VINTF, SELinux, init, image-packaging and compatibility errors.
- [ ] Produce a flashable test image and record checksums.
- [ ] Test on SM-G9650 and publish a feature-by-feature report.

## Milestones and acceptance gates

### M0 — Source control / project safety

**Done when:** documentation, reference sources, manifest files, and validation workflow are versioned; no full Android tree, ROM image, private signing key, credentials, or unapproved proprietary binaries are committed here.

### M1 — Reference source lock (current milestone)

Use the pinned LineageOS 20 sources in [lineage-20-reference.xml](../local_manifests/lineage-20-reference.xml) as an integration checkpoint. The release image from 2022 is a behavior reference; the manifest is not a claim that it recreates the exact source revision used to build that image.

**Acceptance gate:**
- All pinned projects clone/fetch successfully.
- `repo sync` completes without missing projects.
- `lineage_star2qltechn-userdebug` is recognized.
- All direct and transitive dependencies are documented.
- A build attempt finishes or its first blocker is captured with actionable logs.

### M2 — Android 16 port branch

Start from official LineageOS `lineage-23.2` and port/update the device-specific tree, shared tree, kernel integration and vendor configuration. Keep the working reference set separate; never silently mutate the only baseline.

**Acceptance gate:**
- Target product parses and builds far enough to produce a defined image.
- No unresolved duplicate project paths or invalid manifest refs.
- Each compatibility change has a patch, build result, and rationale.

### M3 — Boot and radio bring-up

Test first boot, repeat boot, recovery access, SIM registration, calls, SMS, mobile data, Wi-Fi and Bluetooth. Capture exact build fingerprint, bootloader/baseband information, logs and failure conditions.

### M4 — Daily-use validation

Test audio/microphone, cameras, sensors, fingerprint, GNSS, NFC if available, charging, battery reporting, sleep/wake, encryption, thermal behavior, and stability over repeated reboots and ordinary daily use. Document failures honestly.

## Build infrastructure

Standard GitHub-hosted runners are not sufficient for a full Android source tree due to their disk and memory limits. Use GitHub Actions for validation/orchestration and a suitably sized larger or secured self-hosted runner for ROM builds. A practical planning target is a machine with at least 64 GB RAM and ample high-speed storage; roughly 1 TB or more of free workspace/storage provides useful headroom for source, output, and caches. Validate exact disk and memory needs before paying for a runner.

Do not expose a self-hosted runner to untrusted pull requests. Keep credentials/signing secrets out of logs and do not enable release signing until the source and build pipeline are trusted.

## Current risks / blockers

1. Historical source sets use several forks whose `lineage.dependencies` do not always point to matching branches. Pin dependencies rather than combining by repository name alone.
2. The release and current source branch are not necessarily the exact same revision: the S9+ device-tree branch commit inspected is dated after the release asset upload.
3. Samsung vendor trees may contain proprietary files. Do not copy them into this public project unless redistribution rights are clear; document firmware origin and extraction method.
4. Android 16 has no verified device-specific target in this project's audit yet. Kernel and vendor compatibility must be proven, not assumed.
5. Hardware testing requires the actual SM-G9650; cloud CI cannot validate telephony, Bluetooth or other physical-device behavior.

## Working convention

One milestone at a time. Every proposed change should include:
- exact source commit(s)
- failure or reason being addressed
- expected result
- build/check result
- real-device test result when applicable

Maintain a known-buildable baseline branch and keep experiments isolated from it.
