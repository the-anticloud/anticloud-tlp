# Independent Insurance — TLP

**Project:** TLP  
**Category:** LAPTOPS  
**Upstream:** see BENCH.json  
**Pinned commit:** `91d7bca65721928af30c6a2bb1a1f517f7862964`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `51c3daf63f257859b263a541de8e9da6e8515bd3c1e57414ce59c123cd03d7ce`  
**Date:** October 2026

## Why AI-specific cover matters

Deploying AI in a regulated sector creates liability surfaces that ordinary
technology cover does not reach: inference liability, audit-trail liability,
data-breach liability and IP-infringement liability.

## How this project's architecture reduces insurable risk

| Risk | Cloud AI | TLP with AIOSS |
|---|---|---|
| Audit-trail loss | high — vendor-controlled logs | low — append-only chain, verifiable offline |
| Data breach in transit | high — data transits external servers | low — no external endpoint |
| Compliance violation | high — cannot satisfy air-gap requirements | low — structural |
| IP liability | moderate | low — pinned provenance chain |

## Evidence package for an insurer

- AIOSS chain verification for head `51c3daf63f257859b263a541de8e9da6e8515bd3c1e57414ce59c123cd03d7ce`
- The 16-check register with per-check evidence hashes
- Framework control mapping in `BENCH.json`

## Contact

lois@0-1.gg
