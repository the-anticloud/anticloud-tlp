# Build and Test

**Project:** `TLP`
**Upstream:** https://github.com/nicedoc/tlp
**License:** GPL

## Quick Start

```bash
git clone https://github.com/nicedoc/tlp
cd tlp
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B local AI assistant running offline on consumer hardware
2. AIOSS secure boot measurement chain (TPM2-backed)
3. AES-256 full-disk encryption with AIOSS key audit log
4. Single-binary power management and system optimizer
5. Zero-cloud: all AI features work without internet
6. GPU/CPU equalizer: routes inference to discrete GPU when present, CPU otherwise
7. Zero-telemetry: removes all OEM telemetry and Microsoft/Apple cloud sync defaults
8. Open firmware integration: coreboot/UEFI compatibility layer

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
