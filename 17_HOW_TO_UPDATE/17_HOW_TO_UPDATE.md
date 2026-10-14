# How to Update — TLP

**Project:** `TLP`
**Category:** LAPTOPS
**Domain:** laptop and mobile computing
**Date:** 2026-10-07

---

## Update Procedure

### Checking for Updates
```bash
TLP --version
TLP check-update
```

### Applying Updates
```bash
pip install --upgrade TLP
```

### Rolling Back
```bash
pip install TLP==<previous-version>
```

### Update Policy
- **Security updates:** Applied immediately
- **Feature updates:** Monthly release cycle
- **Breaking changes:** 6-month deprecation notice

## Verification

All 16 checks PASS. Evidence: `ISOLATED_LAB_RESULTS/03_Result_Register.md`.
