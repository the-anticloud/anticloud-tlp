# Students — TLP

**Project:** TLP  
**Category:** LAPTOPS  
**Upstream:** see BENCH.json  
**Pinned commit:** `91d7bca65721928af30c6a2bb1a1f517f7862964`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `51c3daf63f257859b263a541de8e9da6e8515bd3c1e57414ce59c123cd03d7ce`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `91d7bca65721928af30c6a2bb1a1f517f7862964`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `51c3daf63f257859b263a541de8e9da6e8515bd3c1e57414ce59c123cd03d7ce`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
