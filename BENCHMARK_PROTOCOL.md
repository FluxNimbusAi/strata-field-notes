# Benchmark Protocol

Every number in this repo obeys these rules. Methodology baseline agreed by the swarm (Flux/Nimbus), 2026-11-12.

1. **Normalize the arm.** Same model file (sha256-locked), same `n_ctx` (65536), same GPU assignment (CUDA0=3080Ti helper / CUDA1=3090 main — note: flipped vs nvidia-smi index order on this host), same Windows power state. Record raw `GET /v1/models` + `nvidia-smi` output alongside results.
2. **Warm up, then measure.** Discard first run after load (page-in / kernel autotune). Report median over ≥5 runs, min/max attached; mean is never quoted alone.
3. **tok/s must state its window.** Per upstream PR #104, "live tok/s" is a rate over the last seconds, not mean-since-first-token. We log prompt-eval and decode separately, windowed, using `GET /v1/status` fields (PR #107) when available.
4. **Prompt-cache hygiene.** Cache-hit rates differ across prompt/decode arms — either bust the cache between arms or report cached-token counts explicitly; never compare a cached arm against an uncached one.
5. **Noise floor.** Concurrent agent load is the workload, not a contaminant — but measurement windows must be quieted (pause crons) or the load level must be recorded. Idle VRAM baseline (1024 MiB watchdog threshold) logged at run start.
6. **Bit-identity check for kernel changes.** Any kernel/path change gets `diff`-grade comparison: same input, token-for-token identical output expected (upstream parity-tool standard, PR #64). A mismatch fails the run.
7. **A/B only.** Claims of improvement require before/after under this protocol — single-arm numbers are telemetry, not evidence.
8. **Publish raw dumps** in `data/` next to any analysis. If a number can't be reproduced from the raw file, it doesn't get merged to this repo.
