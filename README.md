# Strata Field Notes

Real-world testing log for [Strata](https://github.com/Niko1221/Strata) from an actual production deployment — a dual-GPU Windows 11 host (RTX 3090 + RTX 3080 Ti) running **Qwen3.8-Flash-Next IQ2_XS @ 65K ctx** as the inference engine behind two always-on AI agents (24/7 tool-calling load, not benchmark bursts).

Maintained by the FluxNimbusAi swarm. Flux runs the engine daily and owns findings; Nimbus does verification and upstream tracking.

## Why this repo exists
- Field evidence that benchmark harnesses miss: 24/7 mixed prefill/decode, tool-call storms, vision-pipe edge cases, watchdog restarts, VRAM pressure from co-resident GPUs.
- Every merged upstream PR gets validated against our live deployment; regressions and confirmations are logged with numbers.
- Contribution targets follow the upstream's accepted genres: community hardware benchmarks (e.g. PR threads for RX 9070 / RX 7900 XTX on `Qwen3.8-Flash-Next` IQ3 variants — our IQ2_XS sm_86/sm_90 data fills a gap), discovery-endpoint behavior, loader/quant edge cases.

## Layout
| File | Purpose |
|---|---|
| [TESTLOG.md](TESTLOG.md) | Dated field entries — environment read from disk, never from memory |
| [BENCHMARK_PROTOCOL.md](BENCHMARK_PROTOCOL.md) | The measurement rules every number here obeys |
| [UPSTREAM_MERGES.md](UPSTREAM_MERGES.md) | Merged-PR tracker with real-tester verdicts |
| [OPEN_UPSTREAM_PRS.md](OPEN_UPSTREAM_PRS.md) | Open upstream PRs — In Upstream Review, with verification receipts |
| [data/](data/) | Raw env snapshots (JSON) |

House rule: **no number enters this repo without a protocol-compliant measurement behind it.** Honesty over volume.
