# Source sync results

Updated: 2026-10-13

## Result: pinned device source projects fetched successfully

Workflow: [Sync pinned G9650 reference source projects](https://github.com/LiuuiL98/G9650-Android-16/actions/runs/38279062681)

The workflow completed successfully. It verified that the LineageOS `lineage-20.0` platform manifest branch exists, fetched each of the five projects from [`lineage-20-reference.xml`](../local_manifests/lineage-20-reference.xml) at its exact pinned commit, and checked essential device identity/configuration fields.

| Project path | Pinned commit | Result |
|---|---|---|
| `device/samsung/star2qltechn` | `ba5bc312b8fd638093b7ac5a0604986342ed29d7` | PASS |
| `device/samsung/sdm845-common` | `c13b15e6392b4d15ae18d8d8ea64560f0ee02f88` | PASS |
| `hardware/samsung` | `65fcb7bb55a35d6751bbe289fcba894fce39becf` | PASS |
| `kernel/samsung/sdm845` | `aa7f013da287f80642982f582f4035fbabd5fe89` | PASS |
| `vendor/samsung/sdm845-common` | `bef4f36124cebc85c12c8cab6e0279895c82f53e` | PASS |

Additional checks passed:
- `PRODUCT_DEVICE := star2qltechn`
- `PRODUCT_MODEL := SM-G9650`
- Device `BoardConfig.mk` selects `lineage_star2qltechn_defconfig`
- The pinned kernel contains `arch/arm64/configs/lineage_star2qltechn_defconfig`
- The selected common-tree and vendor-tree makefiles include the `star2qltechn` codename
- The source-sync report artifact was uploaded by Actions: [download report](https://github.com/LiuuiL98/G9650-Android-16/actions/runs/38279062681/artifacts/11760243120)

## What this does and does not mean

**Completed:** the five pinned device/reference projects are retrievable at the requested revisions and pass the listed static consistency checks.

**Not completed:** the full LineageOS platform checkout, `repo sync`, `lunch lineage_star2qltechn-userdebug`, a full ROM build, and hardware tests. The source sync job is intentionally limited to these five projects; it must not be reported as a full Android platform sync.

## Blocker for the full platform sync

The current repository is under a personal account and the workflow uses a standard public GitHub-hosted runner. GitHub currently documents the standard public Ubuntu runner as 4 CPU, 16 GB RAM, and 14 GB SSD; that disk is insufficient for a full Android platform source checkout plus build outputs. See [GitHub-hosted runners reference](https://docs.github.com/en/actions/reference/runners/github-hosted-runners).

GitHub's larger runners are available to organizations/enterprises on GitHub Team or GitHub Enterprise Cloud. Documented Ubuntu options include 16 CPU / 64 GB RAM / 600 GB SSD and 32 CPU / 128 GB RAM / 1,200 GB SSD. See [larger runners reference](https://docs.github.com/en/enterprise-cloud@latest/actions/reference/runners/larger-runners).

Therefore, the next action for a **full** platform sync is to provision a suitable runner:
1. For a fully GitHub-hosted environment, move the repository into a GitHub organization on an eligible plan, enable a larger runner with enough workspace storage, and grant the repository access to that runner.
2. Alternatively, attach a cloud Linux VM as a secured self-hosted runner. In that case Actions remains the orchestrator, but the actual compute is not hosted by GitHub.

A full platform sync should begin only after the chosen runner's available disk has been checked. Do not launch it on the standard runner and expect it to fit.
