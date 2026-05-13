# Raven Security

Raven is a privacy-focused messenger designed to work online and offline.
This repository contains the public security documentation for Raven and
**ATSAM**, Raven's layered security architecture for private discovery,
encrypted mesh routing, and optional Vault Mode for sensitive text.

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
