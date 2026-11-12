# PR #42 verification attempt — memory telemetry, 2026-11-12T17:38 ET (Flux)

PR #42 claims Windows MEM_LARGE_PAGES never worked on Strata before the fix (privilege not enabled, size not page-rounded). Verification requires either the engine startup log line or a memory-shape proxy.

Live read at 17:38 ET (PID 4148, started 17:32:25 — first process after the planned Windows Update boot):
- WorkingSet64 = 40,810,287,104 B = 38,919.7 MiB
- PrivateMemorySize64 = 75,187,630,080 B = 71,704.5 MiB (matches ~71.7 GB IQ2_XS resident model + kv arena)

Divisibility probe: WS mod 2,097,152 = 1,278,976 ≠ 0; Private mod 2,097,152 = 1,441,792 ≠ 0. Neither is page-rounded — but the rounded region is only the expert arena subregion, not the whole process image, so process-level divisibility is NOT a valid test of the claim. Honest status: **still unclaimed**; proxy inconclusive by construction. The decisive read remains the engine's own startup telemetry line ("expert arena ... large pages (2097152 B)") — no log file on this node captures stdout of the VBS-launched engine. Next vector: capture engine stdout at next natural restart via a wrapper, or ask maintainer whether /v1/status can expose arena page mode.

Also confirmed from /v1/status post-boot (usage evidence for field-notes): engine 0.1.55, cache_max_tokens 65536, MTP active (draft_n 90, accepted 66 → acceptance rate 73.3%), last request decode 135.7 tok/s with 42893 cached prompt tokens — the cached-token hygiene rule directly visible in live telemetry.
