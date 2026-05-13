# Raven Trust Roadmap

Raven is building trust step by step.

We are not asking for blind trust. Below is the public plan that
takes Raven from "early privacy-focused messenger with public
documentation" to "independently reviewed cryptographic stack".

Each stage has a definition of done, a current status, and clear
exit criteria. Status updates land in this file and in
[`IMPLEMENTATION_STATUS.md`](IMPLEMENTATION_STATUS.md).

---

## Stage 1 — Public security overview

**Status:** ✅ Complete (May 2026)

We publish a public overview of ATSAM, Raven's layered security
architecture. The overview describes what Raven protects, what
Raven does not protect, the five-layer stack, the threat model, an
honest comparison to existing systems, and explicit limitations.

**Done when:**
- ATSAM overview is publicly hosted on the Raven website.
- A Markdown copy lives in this repository.

**Artifacts:**
- [`ATSAM_PUBLIC_OVERVIEW.md`](ATSAM_PUBLIC_OVERVIEW.md)
- [`docs/ATSAM_Public_Security_Overview_May_2026.pdf`](docs/ATSAM_Public_Security_Overview_May_2026.pdf)
- <https://raven-messager.com/atsam>

---

## Stage 2 — Public threat model

**Status:** 🔄 In progress (May 2026)

We document, in this repository:

- The assets we protect.
- The adversaries we defend against.
- The adversaries we do not fully defend against.
- The security claims per layer, with their status and limitations.

**Done when:**
- [`THREAT_MODEL.md`](THREAT_MODEL.md) lists every adversary class
  with a defense statement OR an explicit "not yet defended"
  acknowledgement.
- [`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md) has a per-layer table
  matching the product user interface.

**Artifacts:**
- [`THREAT_MODEL.md`](THREAT_MODEL.md)
- [`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md)

---

## Stage 3 — Open cryptographic test vectors

**Status:** 📅 Planned (target: Q3 2026)

We will publish deterministic test vectors for the ATSAM
components in this repository so researchers and developers can
verify behaviour without needing the full Raven app.

**Done when:**
- Test vector JSON files in [`test-vectors/`](test-vectors/) cover:
  - Pairing root derivation (fake seeds → expected sub-keys)
  - BBE discovery beacon construction
  - Live confirmation challenge / response
  - Ghost Route recipient tag generation
  - Vault Mode (PV-RAW) encryption + Wegman-Carter MAC

- A README explains how to plug the vectors into an alternative
  implementation.

- All inputs are clearly labelled as **fake test keys**. No production
  material is in the vectors.

**Artifacts (planned):**
- `test-vectors/pairing_root_vectors.json`
- `test-vectors/bbe_discovery_vectors.json`
- `test-vectors/live_confirmation_vectors.json`
- `test-vectors/routing_tag_vectors.json`
- `test-vectors/vault_mode_vectors.json`

---

## Stage 4 — Open cryptographic reference implementation

**Status:** 📅 Planned (target: Q4 2026)

We plan to open-source a minimal cryptographic reference
implementation in a separate repository, `raven-crypto-core`. It
will deliberately be **smaller than the Raven app** — only the
ATSAM crypto primitives, with the test vectors above as its
self-test suite.

Scope of `raven-crypto-core`:

- BBE beacon generation and detection.
- ATSAM key derivation with the published labels.
- Vault Mode (PV-RAW) reference implementation.
- Wegman-Carter MAC reference implementation.
- Routing tag derivation.
- Live confirmation challenge-response.

**Done when:**
- The reference implementation passes every test vector in
  this repository.
- The reference implementation has its own SECURITY.md +
  DISCLOSURE_POLICY.md.
- It is *not* coupled to the Raven app's transport, UI, or
  product code.

**Language preference:** Rust first (better audit reach +
cross-platform reuse), Swift second (Raven is iOS-first).

---

## Stage 5 — Independent security audit

**Status:** 📅 Planned (target: 2027 H1)

We plan to commission independent third-party review of:

- The ATSAM cryptographic core.
- The reference implementation.
- High-risk implementation areas in the Raven app
  (pad-storage discipline, ratchet recovery, crash safety).

We commit to publishing audit findings — including unflattering
ones — in the [`audits/`](audits/) folder of this repository.

**Done when:**
- An auditor's report is hosted in `audits/` with a date,
  scope, and fix status for every finding.

---

## Stage 6 — Community review and bug bounty

**Status:** 📅 Planned (after Stage 5)

We will invite broader security-community review before
recommending Raven for high-risk use cases. This includes:

- A formal bug bounty programme with published payouts.
- An advisory list (acknowledgement of researchers who helped).
- Periodic re-audits as the protocol evolves.

**Done when:**
- Bounty programme is live with published scope and rates.
- At least one external researcher has been credited in a
  release advisory.
- A retest cycle is scheduled (every major release).

---

## Anti-goals

We explicitly will not:

- Claim "unbreakable" or "military-grade" security on any layer.
- Hide negative audit findings.
- Open-source the full Raven application or backend before we have
  hardened anti-abuse and anti-spam systems.
- Promise post-quantum security in marketing without specifying
  the hybrid construction and the post-quantum mechanism in
  documentation.

## A note on pace

We would rather ship Stage N+1 honestly in twelve months than
ship a half-baked "we're audited" claim in three. The pace of
this roadmap reflects that.
