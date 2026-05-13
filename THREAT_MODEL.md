# Raven Threat Model

This document explains, in plain language, what Raven is designed
to protect and what Raven explicitly does not protect. Honest scope
statements are part of the security boundary, not a marketing
afterthought.

This is a public-facing document. For the per-layer security
claims with status and limitations in one table, see
[`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md).

---

## 1. Assets we protect

Raven exists to protect:

1. **Message content.** What people say to each other.
2. **Nearby-peer discovery privacy.** Who is in radio range of
   whom, in the eyes of someone who is not your friend.
3. **Recipient identity at the mesh layer.** When messages travel
   through a stranger's phone, the stranger should not learn whose
   message they are forwarding.
4. **Long-term content secrecy for opted-in messages.** If a user
   marks a message as Vault Mode, that message should remain
   confidential even against future computational advances,
   provided the pad conditions are met.

Raven does **not** primarily exist to protect:

- The fact that some traffic exists.
- Aggregate timing or volume of radio emissions.
- The user's identity from an operating system or a device that
  the user has already lost physical control over.

---

## 2. Adversaries we defend against

For each adversary class below, we name what the adversary can do
in practice and how Raven responds.

### 2.1 Nearby stranger sniffing radio

**Can do:** Sit next to you in a café with a Bluetooth or Wi-Fi
sniffer and capture every frame.

**Raven response:** Discovery beacons contain a fixed-size shuffled
field of slot values, each indistinguishable from a uniform random
string to anyone without the per-pair discovery key. The beacon
does not contain a name, a phone number, a public key, or any
stable identifier.

**What still leaks:** That *some* Raven device is transmitting.
Volume and timing of frames remain visible.

### 2.2 Replay attacker

**Can do:** Record your beacons at one location and re-broadcast
them elsewhere later, hoping to make your friend's phone show "you
are nearby" when you are not.

**Raven response:** A successful discovery is only a *candidate*.
Before the UI says "verified live peer", the live-confirmation
layer issues a fresh challenge and verifies a response computed
with the per-pair `K_live` key. A recorded beacon alone cannot
satisfy a fresh challenge.

**What still leaks:** Active relay attacks (fast forwarding of
the live challenge between distant devices) are not fully
defeated. See section 3 below.

### 2.3 Mesh relay carrying messages

**Can do:** Forward envelopes between users without being part of
either user's trust circle.

**Raven response:** Each envelope carries a per-message routing
tag derived from the recipient's `K_route`. The tag rotates per
message, so successive envelopes cannot be linked to the same
recipient by stable identifiers. Relays see only the rotating tag
and the opaque ciphertext.

**What still leaks:** The fact that an envelope passed through
this relay, with its size and timestamp.

### 2.4 Centralised server or cloud provider

**Can do:** Store metadata, log access patterns, or be compelled
to disclose what it sees.

**Raven response:** For offline traffic, no central server is
involved. For bridged traffic, the server sees only the rotating
recipient tag plus opaque ciphertext, not stable identifiers.

**What still leaks:** Server-side timing and traffic shape.
Defending against that requires cover traffic, which is on the
roadmap (Stage 7 of the protocol roadmap), not in the current
build.

### 2.5 Future quantum attacker

**Can do:** Run Shor-style algorithms once large-scale quantum
computers exist, defeating classical elliptic-curve cryptography.

**Raven response:** The pairing layer uses **both** X25519 and
ML-KEM-768. An attacker would have to defeat both to recover the
shared root. ML-KEM-768 is a NIST FIPS 203 standard, designed for
resistance to known quantum algorithms.

**What still leaks:** The pairing layer itself is computational
security. Vault Mode, when used, is information-theoretic *under
the pad conditions* — content protected by a properly used pad
remains secret regardless of future computational advances.

### 2.6 Backup rollback attacker

**Can do:** Restore an older device backup to try to reuse a pad
slice or a ratchet state that has already been spent.

**Raven response:** Atomic cursor persistence and monotonic
state guards. Consumed pad regions are wiped before the message
leaves the device; restoring an older backup does not bring them
back.

---

## 3. Adversaries we do NOT fully defend against

We list these explicitly so users can make informed decisions.

### 3.1 Endpoint compromise

A jailbroken phone, sophisticated malware with root access, or a
device that has been physically captured. Once an attacker can run
arbitrary code with the user's privileges, no protocol can prevent
them from reading whatever the user can read. Endpoint security is
the user's and the operating system's responsibility.

### 3.2 Global passive observer of all radio

An adversary with the ability to monitor every Bluetooth and
Wi-Fi packet over a wide area can still see traffic patterns: who
emits when, who emits how much, who is co-located with whom. Even
with rotating identifiers, the *shape* of traffic remains visible.
Defeating this requires cover traffic, padding, and traffic
shaping, which is planned future work.

### 3.3 Active mafia-fraud relay attack

Two high-speed adversary-controlled devices, one near Alice and one
near Bob, can in principle forward the live-confirmation challenge
faster than the physical distance would allow honest peers to do.
This makes the UI think Alice and Bob are nearby when they are
actually far apart. Defeating this fully requires *distance bounding*
based on radio signal characteristics, which is planned future work.

### 3.4 Social-engineering attacks at pairing time

Raven provides a strong cryptographic pairing primitive, but it
cannot prevent a user from pairing with the wrong person — for
example, an impostor who looks like a friend in a video call.
Identity verification at pairing time (in-person QR code exchange
or short-authentication-string comparison) remains the user's
responsibility.

### 3.5 Compromised pairing partner

If your peer's device is compromised, they can read everything that
arrives on their side. Raven is a two-party privacy tool: the
weakest endpoint sets the floor.

### 3.6 Coercion and lawful access at the device

If a user is compelled to unlock their device — for example, at a
border or under physical threat — Raven cannot defend against that
contextually. We provide hardware-bound key storage (Secure Enclave
on iOS) so that a device clone without the unlock factor is useless,
but a user who unlocks the device under duress reveals what the
device can show.

---

## 4. Risk stratification

We classify properties into three tiers, mirroring the public
overview document:

### 4.1 Cryptographically defended

Properties for which we have or are working toward a formal
security argument under standard assumptions. As of May 2026 this
includes:

- Pairing indistinguishability under hybrid (X25519 + ML-KEM-768).
- Discovery indistinguishability against a passive observer
  without the per-pair discovery key.
- Live-confirmation replay resistance under a fresh-challenge
  assumption.
- Routing-tag unlinkability across envelopes.
- Vault Mode content confidentiality under pad-uniformity and
  pad-uniqueness assumptions.

### 4.2 Operationally defended

Properties that depend on correct implementation discipline. These
have engineering tests but not cryptographic theorems.

- Pad storage integrity.
- Cursor non-reuse across crashes and rewrites.
- Backup-rollback prevention.
- Atomic state machine transitions on send/receive.

### 4.3 Acknowledged residual risk

Properties we do not yet protect against. Their presence in this
list is intentional — overclaiming is a worse defect than
acknowledging a gap.

- Global radio traffic analysis (volume, timing, shape).
- Active distance-fraud relay attacks between live-confirmation
  parties.
- Side-channel attacks against the cryptographic implementation
  (cache, electromagnetic, fault injection).

---

## 5. Future work

The roadmap below tracks where we expect each acknowledged
residual risk to move from section 3 into section 2 over time.

| Risk | Planned defense |
| --- | --- |
| Global radio traffic analysis | Cover traffic and traffic shaping (ATSAM Layer 7, planned) |
| Active relay attacks on live confirmation | Distance bounding based on radio signal characteristics (ATSAM Layer 6, planned) |
| Side-channel resilience | Reference implementation hardening + audit |
| Identity verification at pair time | UX improvements + safer onboarding flows |

These items are part of the public trust roadmap; see
[`TRUST_ROADMAP.md`](TRUST_ROADMAP.md).

---

## 6. How we treat this document

This file is part of the security boundary, not marketing copy.

If a finding shows that a claim in this document does not match the
behaviour of the shipped product, we will:

1. Update the document to match reality first.
2. Then fix the implementation.
3. Acknowledge the gap publicly.

In that order. The honest version of the threat model always wins.
