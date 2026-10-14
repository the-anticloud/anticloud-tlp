# Educators — TLP

**Project:** TLP  
**Category:** LAPTOPS  
**Upstream:** see BENCH.json  
**Pinned commit:** `91d7bca65721928af30c6a2bb1a1f517f7862964`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `51c3daf63f257859b263a541de8e9da6e8515bd3c1e57414ce59c123cd03d7ce`  
**Date:** October 2026

## Teaching with TLP

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `51c3daf63f257859b263a541de8e9da6e8515bd3c1e57414ce59c123cd03d7ce` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.
