# Reproduction — TLP

1. Environment: Windows, Python 3.12.10, runner version 1.0.0
2. `cd anticloud/`
3. `python tools\run_bench.py --quiet`  (exit 0 = all 16 PASS)
4. Compare `anticloud/BENCH.json` SHA3-256: `c6bd0160cc034bbe9ef318a7a87070202f779f0494b93e71dfa7a57ecd627826`

The `anticloud/` overlay is a standalone copy of the anticloud_reference tree; the 16 checks run against it via `tools/run_bench.py`.
