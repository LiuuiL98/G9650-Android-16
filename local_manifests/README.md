# Local manifest staging

This directory is reserved for small, reviewable repo manifest overlays used by the Android source checkout.

Before adding `roomservice.xml` or other manifest files:
1. Confirm each repository URL and branch exists.
2. Pin critical repositories to compatible branches or commit revisions when appropriate.
3. Check whether the selected Android manifest already includes any dependency, to avoid duplicate project paths.
4. Keep proprietary binaries out of this repository unless redistribution is explicitly permitted.
5. Run `repo sync` validation in the build environment before attempting a ROM build.

No target manifest is committed yet because the exact Android 16-compatible device/common/kernel source revisions still need to be verified.