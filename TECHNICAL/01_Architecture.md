# Technical Architecture — TLP

**Upstream:** [https://github.com/nicedoc/tlp](https://github.com/nicedoc/tlp)
**License:** GPL
**Category:** LAPTOPS
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Linux power management for laptops

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local AI assistant running offline on consumer hardware
2. AIOSS secure boot measurement chain (TPM2-backed)
3. AES-256 full-disk encryption with AIOSS key audit log
4. Single-binary power management and system optimizer
5. Zero-cloud: all AI features work without internet
6. GPU/CPU equalizer: routes inference to discrete GPU when present, CPU otherwise
7. Zero-telemetry: removes all OEM telemetry and Microsoft/Apple cloud sync defaults
8. Open firmware integration: coreboot/UEFI compatibility layer

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_tlp.spec` or `go build -o tlp`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |