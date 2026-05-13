# Implementation Status

This file tracks the engineering status of each ATSAM layer. It is
updated alongside the iOS / cross-platform builds and is meant to
reflect what is actually shipped, not what is planned.

For the user-facing claim per layer, see
[`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md). For the public trust
plan, see [`TRUST_ROADMAP.md`](TRUST_ROADMAP.md).

Last updated: **2026-05-13**

---

## ATSAM layer status

| Layer | Spec | Internal reference (Swift) | Public reference (`raven-crypto-core`) | Test vectors | Audit |
| --- | --- | --- | --- | --- | --- |
| 1. Pairing (X25519 + ML-KEM-768) | ✅ Complete | ✅ Stage 1 shipped (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 2. Private Discovery (BBE) | ✅ Complete | ✅ Stage 2 shipped (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 2b. Discovery partitioning | ✅ Complete | ✅ Stage 3 shipped (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 3. Live Confirmation | ✅ Complete | ✅ Stage 4 shipped (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 4. Encrypted Mesh Routing | ✅ Complete | ✅ Stage 5 shipped (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 5. Vault Mode (PV-RAW) | ✅ Complete | ✅ Storage + emit/decrypt shipped (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 5b. Wegman-Carter MAC | ✅ Complete | ✅ Shipped with Vault Mode (iOS) | ⏳ Planned | ⏳ Planned | ⏳ Planned |
| 6. Distance bounding | 🔬 Research | — | — | — | — |
| 7. Cover traffic | 🔬 Research | — | — | — | — |

**Legend:**

- ✅ Complete / shipped.
- 🔄 In progress.
- ⏳ Planned.
- 🔬 Research / not yet specified.
- — Not applicable yet.

---

## Trust-roadmap stage status

Mirrors [`TRUST_ROADMAP.md`](TRUST_ROADMAP.md).

| Stage | What | Status |
| --- | --- | --- |
| 1 | Public security overview | ✅ Complete (May 2026) |
| 2 | Public threat model | 🔄 In progress (this repository, May 2026) |
| 3 | Open cryptographic test vectors | ⏳ Planned, Q3 2026 |
| 4 | Open cryptographic reference implementation (`raven-crypto-core`) | ⏳ Planned, Q4 2026 |
| 5 | Independent third-party audit | ⏳ Planned, 2027 H1 |
| 6 | Community review + bug bounty | ⏳ Planned, post-Stage 5 |

---

## Engineering hygiene we track

These are operational items that affect security but are not
cryptographic theorems. We list them because they belong inside
the security boundary.

| Item | Status | Notes |
| --- | --- | --- |
| Atomic pad cursor persistence | ✅ Shipped | Persisted before envelope is handed to transport |
| Backup-rollback prevention | ✅ Shipped | Monotonic version + bitmap-based replay rejection |
| Pad-byte zeroization after consume | ✅ Shipped | Two-pass overwrite (0xFF / 0x00) |
| Constant-time MAC comparison | ✅ Shipped | OR-accumulated XOR; no early-exit branch |
| Fail-closed default for missing PQ keys | ✅ Shipped | `nonHybridFallbackRefused` unless caller opts in |
| Per-direction key separation | ✅ Shipped | Distinct sub-keys per send/receive direction |
| Reproducible builds | ⏳ Planned | Source SHA-256 + Mach-O SHA-256 + bundle SHA-256 |

---

## How to read this file

A `✅ Complete (iOS)` cell for an "Internal reference" column
means that ATSAM layer ships in the production Raven iOS app under
its corresponding feature flag. It does **not** mean the layer has
been independently audited. Audit status is tracked separately in
the "Audit" column and in the [`audits/`](audits/) folder.

A `⏳ Planned` cell with no date attached means we have not yet
committed a date publicly. Dates appear in
[`TRUST_ROADMAP.md`](TRUST_ROADMAP.md) once committed.
