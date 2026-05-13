# Raven Security Claims

This document is the single short answer to the question
"what does Raven actually claim?". Every cell is meant to match
the product user interface verbatim. If the product UI ever
overclaims relative to this table, the UI is wrong, not the table.

For the longer threat model, see
[`THREAT_MODEL.md`](THREAT_MODEL.md). For the public overview,
see [`ATSAM_PUBLIC_OVERVIEW.md`](ATSAM_PUBLIC_OVERVIEW.md).

---

## Per-layer claim table

| Layer | Claim | Status | Limitation |
| --- | --- | --- | --- |
| **1. Pairing** | A shared root secret is established using a hybrid of classical X25519 and post-quantum ML-KEM-768. Defeating only one half is not enough to recover the root. | Designed; reference implementation in progress; needs audit | Computational security, not information-theoretic. Pairing-time identity verification is the user's responsibility (QR comparison or short-authentication-string). |
| **2. Private Discovery** | Per-pair beacons hide stable identity from strangers. Beacons look uniformly random to anyone without the per-pair discovery key. | Designed; reference implementation in progress | Discovery is **candidate** only. Timing, volume, and presence of a Raven device remain visible. False-positive matches are filtered by Live Confirmation. |
| **3. Live Confirmation** | A successful response proves the responder currently holds the pair key and is in radio range at the time of the challenge. Helps prevent replay-based false presence. | Designed; reference implementation in progress | Does not by itself defeat active relay attacks where an adversary forwards the challenge faster than physical distance would allow honest peers. Distance bounding is planned future work. |
| **4. Encrypted Mesh Routing** | Per-message routing tags reduce linkability across envelopes for the same recipient. Mesh relays do not see usernames, phone numbers, or stable recipient identifiers. | Designed; reference implementation in progress | Does not hide *traffic shape*: size, count, timing, and radio location remain visible to a global passive observer. Cover traffic is planned future work. |
| **5. Vault Mode** | Selected high-sensitivity text messages can be protected with a one-time pad. Conditioned on the pad being uniform, secret, used exactly once, and consumed bytes being wiped, the ciphertext leaks zero information about the plaintext. | Designed; reference implementation in progress; experimental | Text and small structured messages only. Pad establishment, storage, and crash safety are computational and engineering-dependent. Vault Mode does not hide message size (in coarse buckets) or social-graph metadata. |

---

## Claim status legend

- **Designed.** Specification is complete with a reasoned security
  argument.
- **Reference implementation in progress.** A clean-room reference
  exists (or will exist in `raven-crypto-core`) for the security
  community to review.
- **Needs audit.** Recommended for independent review before being
  recommended for high-risk use cases.
- **Experimental.** Available but not yet recommended as a default
  mode. Vault Mode falls in this tier.

---

## What we explicitly do not claim

These are not claims we make. If you see Raven marketing material
making any of these claims, the marketing material is wrong.

- "Raven is unbreakable."
- "Raven is 100% secure."
- "Raven hides all metadata."
- "Raven defeats all known and unknown attacks."
- "Vault Mode is quantum-proof for everything."
- "Raven is anonymous against all observers."
- "Raven is military-grade encryption."
- "Raven cannot be hacked."

If a Raven employee, document, or social media post uses any
phrase like the above, please report it via
`security@raven-messager.com` so we can correct it. Cleaning up
overclaiming is part of the security boundary.

---

## How claims are versioned

When a claim changes — for example, when a future ATSAM revision
adds distance bounding or cover traffic — we update this table and
note the change date below.

| Date | Change |
| --- | --- |
| 2026-05-13 | Initial publication. Five layers documented. |

When this table changes, the in-app security badges and the
public website are expected to update within the same release.
