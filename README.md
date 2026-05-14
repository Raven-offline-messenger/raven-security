# Raven Security · ATSAM Protocol

> **ATSAM Protocol** (Anchored Transient Stealth Authentication Mechanism)
> is the open-design security protocol behind [Raven Messenger][raven-site].
> It is a five-layer stack — post-quantum hybrid pairing, private peer
> discovery, live device confirmation, encrypted routing with rotating
> per-message tags, and optional Vault Mode (one-time-pad content protection)
> — that applies uniformly to Raven's three communication paths: offline
> Bluetooth mesh, bridge handoff between mesh and internet, and online
> server-routed delivery.

[raven-site]: https://raven-messager.com/atsam

This repository (`raven-security`) is the **public documentation** side of
ATSAM: the [ATSAM Public Security Overview][overview-pdf] (PDF), the
[threat model](THREAT_MODEL.md), per-layer
[security claims](SECURITY_CLAIMS.md), the
[trust roadmap](TRUST_ROADMAP.md), a
[responsible-disclosure policy](SECURITY.md), and test-vector scaffolding.

[overview-pdf]: https://raven-messager.com/assets/ATSAM_Public_Security_Overview_May_2026.pdf

Raven is early. We do not ask users to trust us blindly. The goal is to
earn trust through public documentation, clear threat models, open
cryptographic components, independent audits, and security-community
review.

## What is ATSAM, in one paragraph

**ATSAM** stands for **Anchored Transient Stealth Authentication Mechanism**.
It anchors trust to a paired-device root secret, derives transient per-message
keys and routing tags from that root, and minimises observable identity
metadata at every layer. Concretely, ATSAM bundles five layers:

1. **Post-quantum hybrid pairing** — X25519 elliptic-curve key exchange
   combined with **ML-KEM-768** (NIST FIPS 203). Both contributions are
   mixed into the root secret, so the pairing is secure as long as either
   primitive holds.
2. **Private peer discovery** — bilateral beacon encryption so paired
   devices recognise each other while making discovery beacons look like
   random noise to everyone else.
3. **Live device confirmation** — a fresh challenge-response exchange
   confirms a peer is genuinely present, defeating replayed-beacon attacks.
4. **Encrypted routing** — every message envelope carries a rotating
   128-bit recipient tag derived per-message from the pair's routing key.
   The same construction protects mesh hops, bridge handoffs, and online
   server delivery, so a forwarder on any path sees only opaque bytes.
5. **Vault Mode (optional)** — one-time-pad content protection for selected
   high-sensitivity text messages. When the OTP conditions are met (random,
   secret, long enough, never reused), it can provide information-theoretic
   secrecy.

ATSAM ships in **Raven Messenger v1.7+**. The protocol's design rationale,
threat model, claim list, and intended limits are all documented in this
repository.

Raven is early. We do not ask users to trust us blindly. Our goal is to
earn trust through public documentation, clear threat models, open
cryptographic components, independent audits, and security-community
review.

## How this repository fits with the rest of Raven

Raven is **already open source**. The full application source code,
including the iOS / Mac client, the server, and the BLE mesh stack,
lives at:

➡️ **[`Raven-offline-messenger/RAVEN`](https://github.com/Raven-offline-messenger/RAVEN)**
&mdash; licensed under AGPL-3.0.

That repository contains the actual code that runs on users' devices.

**This** repository (`raven-security`) is the *documentation* side of
Raven's trust posture. It is intentionally smaller and more focused
than the main repo:

- **`Raven-offline-messenger/RAVEN`** &mdash; the running code (AGPL-3.0).
- **`Raven-offline-messenger/raven-security`** &mdash; threat model,
  per-layer security claims, trust roadmap, responsible disclosure,
  test vector scaffolding, ATSAM public overview (CC-BY-4.0).

A security researcher who wants to *audit Raven's actual binary* should
start at the main repo. A researcher who wants to understand *what
Raven claims to defend and what it doesn't* should start here.

A planned third repository, **`raven-crypto-core`**, will host a
clean-room cryptographic reference implementation of the ATSAM
primitives, separate from the full app. See
[`TRUST_ROADMAP.md`](TRUST_ROADMAP.md) Stage 4.

---

## What is ATSAM?

ATSAM is Raven's layered security protocol. It combines:

- **Post-quantum hybrid pairing** — classical X25519 plus ML-KEM-768
- **Private peer discovery** — paired devices find each other in radio range without broadcasting names, phone numbers, or stable public keys
- **Live device confirmation** — fresh challenge-response binds a discovery match to actual presence, not a recording
- **Encrypted mesh routing** — per-message rotating tags so mesh relays cannot link successive envelopes to the same recipient
- **Optional Vault Mode** — one-time-pad content protection for high-sensitivity text messages, under strict pad conditions

Each layer has a specific security goal and specific limitations. The
goals and the limitations are documented per layer in
[`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md) and in the public overview
PDF in [`docs/`](docs/).

---

## Documents in this repository

| File | What it covers |
| --- | --- |
| [`ATSAM_PUBLIC_OVERVIEW.md`](ATSAM_PUBLIC_OVERVIEW.md) | Markdown version of the public ATSAM overview (the same document also lives as a PDF in `docs/`) |
| [`THREAT_MODEL.md`](THREAT_MODEL.md) | Assets we protect, adversaries we defend against, adversaries we do not fully defend against, residual risks |
| [`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md) | Per-layer security claim, status, and limitation in one table |
| [`SECURITY.md`](SECURITY.md) | How to report vulnerabilities + current security status |
| [`DISCLOSURE_POLICY.md`](DISCLOSURE_POLICY.md) | Responsible disclosure guidance for researchers |
| [`TRUST_ROADMAP.md`](TRUST_ROADMAP.md) | Six-stage public plan for earning trust over time |
| [`IMPLEMENTATION_STATUS.md`](IMPLEMENTATION_STATUS.md) | Which ATSAM layers are designed, implemented, tested, audited |
| [`docs/ATSAM_Public_Security_Overview_May_2026.pdf`](docs/ATSAM_Public_Security_Overview_May_2026.pdf) | 15-page printable overview, same content as the Markdown |
| [`test-vectors/`](test-vectors/) | (Planned) deterministic test vectors for ATSAM components |
| [`audits/`](audits/) | (Planned) third-party audit reports when they exist |
| [`research/`](research/) | (Planned) external research notes and collaboration material |

---

## Current maturity

Raven is not yet in the same trust tier as long-established,
independently audited secure messengers. We are working toward that
through:

- Public security documentation (this repository)
- Open cryptographic test vectors (planned)
- A separate `raven-crypto-core` reference implementation (planned)
- Independent third-party audits (planned)
- Security-community review and responsible-disclosure intake

The [`TRUST_ROADMAP.md`](TRUST_ROADMAP.md) tracks our progress.

If you are evaluating Raven for a high-risk use case today, please
read [`THREAT_MODEL.md`](THREAT_MODEL.md) and
[`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md) carefully, and prefer
established audited tools until our audit work is complete.

---

## Reporting a vulnerability

Please follow [`SECURITY.md`](SECURITY.md) and
[`DISCLOSURE_POLICY.md`](DISCLOSURE_POLICY.md).

In short: email **security@raven-messager.com** with a description,
reproduction steps, and impact. We aim to acknowledge within 7 days.

---

## License

Documentation in this repository is licensed under
[Creative Commons Attribution 4.0 International (CC-BY-4.0)](LICENSE).
You are free to share and adapt the material with attribution.

When the reference implementations land in a separate
`raven-crypto-core` repository, that code will be licensed
separately under a permissive open-source license suitable for code.

---

## Contact

- General security inquiries: `security@raven-messager.com`
- Press / partnership: `info@raven-messager.com`
- Website: <https://raven-messager.com/>
- ATSAM overview on the website: <https://raven-messager.com/atsam>
