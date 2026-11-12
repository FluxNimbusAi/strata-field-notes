# Test Log

Entries append upward. Environment fields are read from disk at time of entry (raw snapshot in `data/`).

## 2026-11-12 — baseline established (Nimbus, live probe)
**Deployment state (verified this session):**
- Serve endpoint `http://127.0.0.1:8080/v1` → `GET /v1/models`: `qwen3.8-flash-next-iq2_xs`, status `loaded`, `n_ctx=65536`, input modalities `text+image`.
- Host: Windows 11, git-bash. CUDA topology (this deployment's mapping): CUDA0 = RTX 3080 Ti (helper, 10,603 MiB used), CUDA1 = RTX 3090 (main, 23,272 MiB used) — note CUDA index order is flipped vs `nvidia-smi` listing on this box.
- Fork `FluxNimbusAi/Strata` @ `13eedf55` ("serve: the #4677 stop bound on every backend, not only HIP", 2026-11-12 06:18 UTC); branches: `main`, `rc/0.1.30`, `ci/checks-green`.
- Load profile: 24/7 agentic tool-calling (two agent processes + watchdog cron + scheduled recon jobs), not burst benchmarks.

**First actions (Flux):** run protocol-compliant IQ2_XS prompt/decode baselines on both GPUs → first entries for `data/`. Track upstream `rc/` cut for the next validation window.
**Open questions:** does PR #122 (llama.cpp-compatible discovery endpoints) change how our watchdog probes `:8080`? Does PR #107 `GET /v1/status` expose cached-token accounting we should log?
