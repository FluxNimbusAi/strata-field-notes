# Upstream Merge Tracker — Niko1221/Strata

Snapshot pulled 2026-11-12 via `gh pr list --state merged`. Verdict column is filled after each PR is exercised on the live deployment (CONFIRMED / REGRESSION / N-A).

**Structural note for real testers:** upstream lands most community work as *squash-style feature-branch merges referencing PR numbers* (e.g. commit `8486336` "Merge f-027-gh-eng-s (#3291 #3955 #3317 #4036 #2893 #3218 #3200)"), with 700+ open PRs in the queue at any time. The merged list is short; the *open queue* is where field-tester signal actually gets triaged. Benchmarks and repro data posted as comments on open PRs move them faster than new issues — our leverage as always-on users.

| PR | Merged | Author | Title | Verdict |
|---|---|---|---|---|
| 126 | 2026-09-29 | Zauberio | Windows: expert load 24x slower from Task Scheduler (0.05 vs 1.42 GiB/s) | TBD — directly relevant: our loader path runs from VBS autostart, not Task Scheduler; capture both numbers |
| 122 | 2026-09-29 | hendrikp | serve: llama.cpp-compatible discovery endpoints | TBD — affects our watchdog probe |
| 118 | 2026-09-29 | maedoc | serve: routing traces from the live server | TBD |
| 113 | 2026-09-29 | Yunado | serve: keep image parts in tool/assistant messages | TBD — we use vision input; regression-test image tool messages |
| 109 | 2026-09-29 | q8atnight | decode: batch verify-window per-token kernels (bit-identical, +~10% decode) | TBD — expect ~10% decode gain; measure per protocol §3 |
| 107 | 2026-09-29 | architectds | serve: cached prompt tokens, llama.cpp timings, GET /v1/status | TBD — becomes our primary metrics source for the log |
| 104 | 2026-09-29 | xyzzing | Monitor live tok/s = rate over last seconds, not mean | CONFIRMED semantics — adopt: log windowed tok/s explicitly |
| 92 | 2026-09-29 | Avicennasis | setup: name missing/short shard with numbers; verify manifest sha256 | TBD |
| 91 | 2026-09-29 | Avicennasis | GGUF reader: refuse duplicate tensor name at open | TBD |
| 89 | 2026-09-29 | dannychirkov | loader: bulk-read expert arena + MTP drafter (MSVC 4095-byte ifstream splits) | TBD |
| 82 | 2026-09-29 | mikicvi | Web app behind path-prefixed reverse proxies | N-A (loopback-only deployment) |
| 81 | 2026-09-29 | mikicvi | Accept sm_80+ GPUs at runtime (matches build guard/README) | CONFIRMED — engine runs on sm_86 + sm_90 in production 24/7 |
| 64 | 2026-09-28 | pipeob0 | parity tool: new-kernel mismatch must fail the run | TBD |
| 44 | 2026-09-28 | pipeob0 | generate: probe real host→device bandwidth for pcie_frac | CONFIRMED 2026-11-13 (probe live: h2d 0.17 / peer 0.35 vs default 0.55; drift across OS update 5.6→6.2 GB/s); upstream comment filed |
| 43 | 2026-09-28 | pipeob0 | kernels: AVX2 multi-token i-quant row kernels | TBD — i-quant path is exactly our IQ2_XS build |
| 42 | 2026-09-28 | pipeob0 | pinned: MEM_LARGE_PAGES on Windows enable+round | FIELD-TESTED 2026-11-13: rounding OK, privilege-enable NOT effective (error 1314 on 30/31 boots, engine 0.1.55); comment filed upstream — see TESTLOG 2026-11-13 |

## Contribution plan (as real-time testers)
1. **IQ2_XS sm_86/sm_90 community benchmark PR** — the stratat.com pull-request wall shows RX 9070 (IQ3_XXS) and RX 7900 XTX (IQ3_S) benchmark threads; nothing for Ada-less Ampere dual-GPU Windows + IQ2_XS. Data collected per BENCHMARK_PROTOCOL, then PR upstream.
2. **Field verdicts on the queue** — comment repro data (with `GET /v1/status` numbers post-#107) on open PRs touching Windows loader / i-quant kernels / pcie_frac — the PRs a 24/7 Windows deployment can see further into than CI.
3. Fork stays synced (`main`, `rc/*`); anything we fix locally gets PR'd upstream, not hoarded — upstream credit back, fork history as recruiter-visible record.
