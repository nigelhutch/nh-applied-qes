# Running a 1-Trillion-Parameter LLM on One 128 GB Ryzen AI MAX+ PC

**QES** demonstrates execution of the ~375 GB **Kimi K2.5** 1-trillion-parameter-class Mixture-of-Experts model on a **single AMD Ryzen AI MAX+ 395 Windows PC with 128 GB unified memory**.

**Published technical white paper — QES v2.7:**  
https://doi.org/10.5281/zenodo.22730031

**Author:** Nigel Hutchinson / NH Applied

---

## Why this matters

AMD publicly demonstrated Kimi K2.5 (UD_Q2_K_XL, 375 GB) using a **four-node cluster** of Framework Desktop systems, each equipped with an AMD Ryzen AI MAX+ 395 and 128 GB of memory.

QES investigates a different deployment question:

> **Can that model class remain executable on one 128 GB Ryzen AI MAX+ system instead of requiring four coordinated machines?**

The answer demonstrated by QES is **yes** — at substantially lower throughput than AMD's multi-node cluster, but with the model remaining locally executable on a single Windows machine.

AMD reference:  
https://www.amd.com/en/developer/resources/technical-articles/2026/how-to-run-a-one-trillion-parameter-llm-locally-an-amd.html

---

## Published Kimi result

The final sustained QES validation recorded:

- **Model:** Kimi K2.5, ~375 GB
- **System:** one AMD Ryzen AI MAX+ 395 with Radeon 8060S
- **Memory:** 128 GB unified memory, tested as 96 GiB GPU / 32 GiB CPU-visible
- **Operating system:** Windows 11 Pro 25H2, build 26200.8524
- **Recorded platform mode:** 85W_BALANCED
- **Sustained generation:** **0.432091702 tokens/s** over 128 generated tokens
- **Frozen CPU baseline:** **0.238894644 tokens/s**
- **Improvement over CPU baseline:** **+80.87%**
- **Reference agreement:** all **129 output token IDs** exactly matched the frozen historical reference sequence
- **GPU expert executions:** **61,440**
- **Authoritative downstream correctness checks:** **7,680**
- **GPU errors:** **0**

The best bounded short-run optimisation result reached **0.437634540 tokens/s**. The sustained 128-token result finished only **1.27% below** that short-run optimum.

---

## Qwen → Kimi: cross-family portability

QES was not developed only around Kimi.

The earlier proof used **Qwen3-235B**, followed by the substantially larger Kimi K2.5 workload:

| Model | Routed MoE layers | Routed experts per layer | Routed-expert payload |
|---|---:|---:|---:|
| Qwen3-235B | 94 | 128 | 137.29 GB |
| Kimi K2.5 | 60 | 384 | 367.30 GB |

Kimi therefore represented roughly a **2.67× increase in routed-expert payload**, while also changing model family and expert topology.

The published claim is deliberately limited: QES has demonstrated **cross-family portability across these two materially different MoE architectures** and is not hard-coded to one model layout. This does **not** establish universal model agnosticism; additional architectures still require mapping, adaptation and qualification.

---

## What QES is

QES is an experimental large-model execution architecture designed for **Mixture-of-Experts models whose routed-expert capacity exceeds the memory normally available for full residency on a single machine**.

At a public architectural level, the work covers:

- bounded expert residency
- NVMe-backed expert access
- GPU-authoritative expert execution
- CPU/GPU execution-path qualification
- integrity-before-execution checks
- continuous correctness validation
- fail-closed behaviour when qualification conditions are not met
- evidence-preserving optimisation and negative-result retention

The Kimi campaign used ordinary internal NVMe storage: a **Samsung SSD 990 EVO Plus 2 TB** as the authoritative expert store and a **YMTC PC41Q-2TB-B** as the Windows system volume and verified secondary expert-store replica. A separate 6 TB USB drive was explicitly excluded from QES acceleration.

---

## What this repository does not publish

This repository is the public project landing point for QES and the published technical evidence.

It intentionally does **not** disclose reconstruction-enabling implementation details such as exact scheduling ratios, expert-record formats, internal state machinery, cache/eviction mechanisms, memory-transfer implementation, launch environment, source paths or build recipe.

The technical white paper defines the public claim boundary and evidence in detail.

**Canonical publication:**  
https://doi.org/10.5281/zenodo.22730031

---

## Scope of the result

QES demonstrates **functional single-node execution and measured optimisation**, not parity with a four-node cluster and not production-chat throughput.

The result is relevant where **local availability, model capability, data locality, system count or controlled on-premises execution** may matter more than interactive generation speed.

No claim is made here of regulatory certification, universal model compatibility, or equivalent performance to AMD's distributed reference implementation.

---

## Rights

Copyright © 2026 Nigel Hutchinson / NH Applied. **All rights reserved.**

No open-source licence is granted by this repository. Third-party model, hardware and product names remain the property of their respective owners.
