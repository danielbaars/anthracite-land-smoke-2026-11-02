# Anthracite 0.6.1 live GitHub landing smoke test

Date: 2026-11-02. Result: the bounded, conflict-free two-PR landing passed against real GitHub.

This report concerns the published npm package `@danielbaars/anthracite@0.6.1` on Node 24.19.0, rather than a source build or a simulated GitHub adapter. The repository contains synthetic text fixtures only. No application repository or active user installation was changed.

## Fixture and consent

The user approved creation of this public repository and four draft PRs, readiness changes for the first two, direct rebase landing and mandatory survivor maintenance, landed-only branch cleanup, closure of the surviving PRs without merging, and retention of this report. No reviewers or issues were added. The repository has no Actions workflows; zero Actions runs occurred.

```text
main → land-base (#1) → land-child (#2) → land-tip (#3)
                   → land-sibling (#4)
                   → land-local-only (unpublished)
main → land-independent (unpublished)
```

The target was the local branch `land-child`. Only PRs [#1](https://github.com/danielbaars/anthracite-land-smoke-2026-11-02/pull/1) and [#2](https://github.com/danielbaars/anthracite-land-smoke-2026-11-02/pull/2) were merge targets. The original checkout was `land-tip`.

## Observations

| Check | Observed result |
| --- | --- |
| All selected PRs still draft | Dry-run refused; every repository file hash, remote head and observed PR field remained unchanged. |
| Only selected PRs marked ready | Dry-run succeeded, disclosed both merges and all mandatory participants, and changed no repository files or remote/PR state. |
| Dirty worktree and explicit `--yes` | Application refused before changes; dirt and all other observed state were preserved. |
| `an land land-child --yes --no-interactive` | Exit 0, 122.27 seconds, two confirmed live merges in dependency order. |
| Merge result | Local and remote main agree; main contains the base and child fixtures, without tip, sibling, local-only or independent additions. |
| Surviving work | Each survivor retains its own binary diff, author and complete commit message, with one commit outside main. |
| Published survivors | Tip and sibling remote heads match local heads; their existing PRs target main and preserve titles, user descriptions, assignees and draft state. Dependency footers no longer list landed PRs. |
| Unpublished work | Local-only and independent branches acquired neither a remote head nor a PR. |
| Independent scope | The independent branch head and metadata ref are byte-for-byte unchanged. |
| Cleanup and checkout | Only landed heads and their metadata were removed locally and remotely; checkout returned to tip, clean, without landing/continuation recovery. |
| Fixture cleanup | PRs #3 and #4 were then closed without merging. Their remote branches remain inspectable. |

## Important boundaries found

### Private repositories can be blocked by the account plan

Before this public experiment, the real GitHub branch-rules endpoint for an existing private test repository returned HTTP 403 with an account-plan upgrade requirement, despite its trunk being reported unprotected. Published 0.6.1 refused its dry-run rather than assuming the missing policy was empty. A complete before/after file-hash comparison confirmed no changes in that isolated probe. No PRs or remote branches were created there, and its settings and visibility were not changed.

That is a verified limitation for that private-repository/account combination. The successful public test does not establish usability for it.

### A detached sibling arm does not follow every later trunk advance

After PR #1 landed, sibling and local-only were replayed onto that first trunk result (`43d44534542543f55f5e9e21308df81828a0ed89`). They then became independent arms. After PR #2 landed, only its remaining descendant tip needed further maintenance, and it was replayed onto final trunk (`9ad3f46ffdddcdd48ac0a344cf7906db3d81b11d`).

Sibling and local-only therefore retain all of their own work but still show `needs restack` against final trunk. The original independent branch also shows that marker, as expected from leaving it untouched. Landing is not a promise to bring every surviving arm onto the final trunk tip; do not append an unreviewed broad restack/submit merely to remove these markers.

## Retained evidence

- [Observed results and artifact identity](evidence/land-0.6.1/results.json)
- [Preflight results](evidence/land-0.6.1/preflight.json)
- [Original branch ownership, patches and commit text](evidence/land-0.6.1/branch-ownership-before.json)
- [PRs before the test](evidence/land-0.6.1/prs-before.json)
- [PRs immediately after landing, before fixture closure](evidence/land-0.6.1/prs-after-land.json)
- [Successful read-only preview](evidence/land-0.6.1/preview.txt)
- [Actual application output](evidence/land-0.6.1/application.txt)
- [Final stack display](evidence/land-0.6.1/topology-after.txt)

## Acceptance limits

This certifies this synthetic, unprotected, same-repository github.com happy path and the stated refusal checks. It does not certify live conflict recovery, required reviews/check publishers, protected branches, queues, forks, enterprise hosting, permission/network failures after partial progress, or arbitrary existing projects. No Anthracite code was changed and no release was made for this experiment.
