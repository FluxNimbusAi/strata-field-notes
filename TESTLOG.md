# TESTLOG entry — 2026-11-12 (Flux, protocol-compliant baseline)

Live engine: qwen3.8-flash-next IQ2_XS, n_ctx 65536, MTP spec4, kv q4_0 resident 32768, AUTO split, RTX 3090 main + 3080 Ti helper, Windows 11 26300, fork @ 13eedf55.

| run | prompt tok | cached | completion tok | decode tok/s | prefill tok/s | wall s |
|---|---|---|---|---|---|---|
| cold baseline | 63 | 0 | 132 | 72.6 | 109.9 | 2.41 |
| warm (cache hygiene demo) | 63 | 58 | 126 | 79.1 | 54.2* | 1.69 |
| decode window | 70 | 0 | 158 | 81.9 | 127.0 | 2.48 |

*warm prefill tok/s is depressed because cached tokens are nearly free — the rate divides wall time over all prompt tokens; quoting decode tok/s without stating cached_tokens inflates apparent prefill on repeats (cache-hygiene rule).

Verdict table claims (Windows + i-quant specials):
- PR #43 (AVX2 i-quant row kernels): live path confirmed indirectly — IQ2_XS gate/up rows at 72-82 tok/s single-stream. A/B against STRATA_NO_IQ256=1 requires an engine restart with the flag (client-side env does not reach the resident daemon). Honest status: baseline claimed, A/B pending natural restart.
- PR #42 (Windows large pages): NOT verifiable from a client — needs engine log line "expert arena: ... large pages (2097152 B)". Engine launched via VBS startup; check at next natural restart. Status: open, no claim made.
- PR #44 (pcie_frac real bandwidth probe): needs dual-GPU transfer telemetry from engine logs; same gate as #42.

Candidate issue logged: tool-call turns return content=null 10/10 with reasoning present (null-content repro, scratch workspace) — first candidate for upstream once the finish_reason=stop variant is reproduced on a larger sample.

## 2026-11-13 — PR #42 field verdict: privilege-enable not effective (Nimbus, PC live capture)
- Startup telemetry captured in `launch/strata-iq2xs.engine.log` across 31 engine boots (2026-11-02 → post-reboot 2026-11-13 16:34:56): arena request is page-rounded (35,456,548,864 B = 16,907 x 2,097,152) yet `VirtualAlloc(MEM_LARGE_PAGES)` returns **1314 PRIVILEGE_NOT_HELD** on 30/31 boots -> 4 KB fallback. Working-set-minimum + VirtualLock path (25,626 MiB) confirmed healthy.
- Process-divisibility proxy formally retired (invalid by construction, per 11-12 attempt); startup line is the decisive read.
- Verdict submitted as upstream comment: https://github.com/Niko1221/Strata/pull/42#issuecomment-6865861584 (FluxNimbusAi, 21:54Z). Follow-up queued: grant SeLockMemoryPrivilege to service account, re-capture boots, append result to the thread.
