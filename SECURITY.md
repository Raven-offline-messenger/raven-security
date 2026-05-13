# Security Policy

Raven is under active development. We welcome responsible security
feedback from researchers, developers, and informed users.

## Current status

Raven has **not yet** completed an independent third-party security
audit. Users should treat Raven as an early privacy-focused messenger,
not yet a replacement for mature audited tools in high-risk scenarios.

The current state of each ATSAM layer is tracked in
[`IMPLEMENTATION_STATUS.md`](IMPLEMENTATION_STATUS.md). The public
trust plan is in [`TRUST_ROADMAP.md`](TRUST_ROADMAP.md).

## Reporting vulnerabilities

If you believe you have found a vulnerability in Raven, ATSAM, or
related cryptographic components, please contact us privately first:

**`security@raven-messager.com`**

Please **do not** publicly disclose vulnerabilities before we have had
a reasonable time to investigate and respond. Coordinated disclosure
keeps users safer.

When reporting, please include:

- A clear description of the issue.
- Steps to reproduce, or a proof-of-concept.
- Potential impact, including which security claim is affected (see
  [`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md)).
- Suggested mitigation, if known.

We aim to acknowledge reports within **7 days** and provide updates
as the investigation progresses.

## Scope

**In scope** for vulnerability reports:

- ATSAM protocol documentation (this repository).
- Public test vectors (when published).
- Open-source cryptographic components (when published).
- Inconsistencies between security claims and stated limitations.
- Inconsistencies between the public ATSAM overview and the
  product user interface.

**Out of scope** for vulnerability reports:

- Spam, abuse reports, or content moderation issues. Please use the
  in-app moderation tools.
- Social engineering against individual users (e.g. tricking a user
  into pairing with the wrong person).
- Physical attacks against an unlocked device.
- Issues that require access to production secrets, internal admin
  systems, or our cloud infrastructure.

## What we do not claim

Raven explicitly does **not** make the following claims. Reports
based on these assumptions will be acknowledged but typically closed
as out-of-scope:

- "Unbreakable" or "impossible to hack" — no honest protocol makes
  this claim.
- "Hides all metadata, including global traffic patterns" — ATSAM
  reduces metadata exposure, but does not defeat a global passive
  observer. See [`THREAT_MODEL.md`](THREAT_MODEL.md).
- "Protects messages on a fully compromised device" — endpoint
  security is the user's and OS's responsibility.
- "Vault Mode is for all message types" — Vault Mode is for small
  text or structured messages only, not large media.

## Acknowledgement

When we publish fixes, we are happy to credit responsible reporters
in release notes (with permission). We do not currently run a paid
bug bounty programme but we plan to introduce one alongside Stage 6
of the [`TRUST_ROADMAP.md`](TRUST_ROADMAP.md).
