# Open Upstream PRs — Niko1221/Strata

Status: **In Upstream Review** (maintainer review is the bottleneck; upstream applies admin-only labels, external authors cannot set them).

| PR | Title | Head | Base | Status | Verification receipt |
|---|---|---|---|---|---|
| [Niko1221/Strata#6116](https://github.com/Niko1221/Strata/pull/6116) | [Engine] serve: expose context_length/max_context_length on /v1/models entries | `f22dbe19d` | main @ `c77572806` | In Upstream Review — mergeable, Intake SUCCESS | [PR_AUDIT_20261117.md](PR_AUDIT_20261117.md); live-verified shape on bundled-runtime mock serve (all 3 keys == `reported_ctx()`); source issue #5945 link comment 6928133046 |
| [Niko1221/Strata#5884](https://github.com/Niko1221/Strata/pull/5884) | [GUI] newSession keeps the model the composer shows | `7c5cfa22e` | v0.2.15 rebase 11-16 | In Upstream Review — 1 file +2/−1 (apps/host/src/lib/store.ts); checks await maintainer workflow approval (empty rollup on new head — not a regression) | [PR_AUDIT_20261117.md](PR_AUDIT_20261117.md); head re-read via REST 2026-11-17 21:00Z |
| [Niko1221/Strata#6193](https://github.com/Niko1221/Strata/pull/6193) | [GUI] control app: /v1/models mirrors engine meta on native rows | `a9e904acb` | main @ `c77572806` | In Upstream Review — open, **dirty**: main moved to `58ee32277` (head 1 behind); rebase owned by drift lane | `compare a9e904acb...58ee32277` → merge-base `c7757280699`, ahead 728 / behind 1; fork branch `tb/6180-models-meta-passthrough` head re-read == `a9e904acb`; re-read 2026-11-18 23:31Z |

Heads re-read from `gh api repos/Niko1221/Strata/pulls/<n>` at doc commit time — no number here is quoted from memory.

## Related internal lanes (fork, pre-upstream)
- [FluxNimbusAi/Strata#1](https://github.com/FluxNimbusAi/Strata/pull/1) — `[Engine] serve: tool-call parser keeps a value whose model skipped its own closer (#5756)` — fork SDLC gate approved 2026-11-17 (run 62); upstream submission awaits maintainer-scope adjudication. Head `ebf412c3dab7`.
- Local-only, never pushed (no remote ref on fork or upstream — REST 404/422 re-read 2026-11-18 23:31Z): `tb/6180-control-meta` @ `bc2aba22a` — pre-amend predecessor of shipped `a9e904acb` in the #6180/#6193 lane, tree byte-identical (`git rev-parse bc2aba22a^{tree}` == `a9e904acb^{tree}` == `8fcefdf4cc27`); authored 2026-11-17 22:54 EST, superseded by the shipped commit at 23:49 EST in the strata-pr clone. No PR to receipt; branch retired.
