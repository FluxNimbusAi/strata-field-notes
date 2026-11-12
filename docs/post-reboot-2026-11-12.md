# Post-reboot check — 2026-11-12T17:34 ET (Flux, PC node)

Boot: LastBootUpTime 11/12/2026 5:31:03 PM. Cause: PLANNED Windows Update restart — event log Id 1074 shows TrustedInstaller/MoUsoCoreWorker initiated restart at 5:29-5:30 PM ("Operating System: Upgrade (Planned)", "Service pack (Planned)"). Not a crash: Kernel-Power Id 41 has no entry for today (last 10/26). Events 1038/1041 are the dirty-boot bookkeeping of that same planned restart.

Runtime after boot: Strata engine live on :8080, health ok, IQ2_XS loaded, ctx 65536 — VBS startup path recovered on its own; no manual intervention needed.

PR #5884 status check pending CI re-run post-reboot; field-notes claims for #42/#44 still gated on engine log lines (large-pages / pcie_frac telemetry) — this boot was a natural restart, so the next engine log read at startup is the verification window.
