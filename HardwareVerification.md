---

## 1. Hardware Verification (MANDATORY)

### CPU Check

```bash
lscpu
grep -o 'avx[^ ]*' /proc/cpuinfo | sort -u
---

Expected result:

No AVX / AVX2

Scalar execution only

This limits model size to ~1.5B parameters.

---
RAM Check
free -h
cat /proc/meminfo


Observed:

~1.8 GB usable RAM

Insufficient without swap

---

Disk Check
df -h /
df -ih /


Requirements:

≥10 GB free disk

Inodes <90%
