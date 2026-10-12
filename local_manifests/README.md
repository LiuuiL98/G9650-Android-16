# Local manifest files

This directory stores small, reviewable repo manifest overlays for source sets.

## Available manifest

- `lineage-20-reference.xml`: a pinned historical LineageOS 20 / Android 13 integration checkpoint. It is **not** the Android 16 target manifest. Initialize the matching LineageOS branch before using it; instructions and commit details are in [reference-source-set.md](../docs/reference-source-set.md).

## Rules

1. Verify every repository, ref and dependency before adding or updating a manifest.
2. Keep source sets branch-compatible; do not mix repositories just because they share the SDM845 name.
3. Pin revisions when reproducing a historical build.
4. Do not commit proprietary files unless redistribution is explicitly permitted.
5. Validate XML and run `repo sync` in a disposable build workspace before attempting a full compile.

The Android 16 / LineageOS `lineage-23.2` manifest has not been committed yet because the exact-device source compatibility has not been verified.
