# Raven Test Vectors

This folder will contain deterministic test vectors for ATSAM
components. The goal is to let independent implementers verify
their work against a public reference without needing the full
Raven application.

**Status (May 2026):** placeholder scaffolding. The vectors will
land in stage 3 of the [Trust Roadmap](../TRUST_ROADMAP.md). The
files in this folder describe the format we will publish; the
actual values will be filled in once the reference implementation
in `raven-crypto-core` is ready.

---

## File overview

| File | What it covers |
| --- | --- |
| `pairing_root_vectors.json` | HKDF root derivation: `(zX, zPQ, transcript) → K_root` |
| `bbe_discovery_vectors.json` | Beacon slot derivation: `(K_BBE, epoch, nonce) → slot` |
| `live_confirmation_vectors.json` | Challenge / response MAC: `(K_live, role, challenge, ...) → MAC` |
| `routing_tag_vectors.json` | Per-message recipient tag: `(K_route, nonce) → recipient_tag` |
| `vault_mode_vectors.json` | PV-RAW XOR + Wegman-Carter MAC under fixed pad slices |

Each JSON file follows the same structure:

```json
{
  "name": "BBE discovery slot",
  "spec": "ATSAM Public Overview §3.2",
  "deterministic": true,
  "inputs_are_fake_test_material": true,
  "vectors": [
    {
      "id": "bbe-001",
      "comment": "32 B all-zero discovery key",
      "input": {
        "K_BBE_hex": "00 ... 00",
        "epoch": 1000,
        "nonce_hex": "00 ... 00"
      },
      "expected": {
        "slot_hex": "TODO"
      }
    }
  ]
}
```

---

## Safety contract

Every value in `inputs` is **fake test material**. This folder
deliberately does NOT contain:

- Real production keys.
- Real user data or contact graphs.
- Real server tokens.
- Real ratchet state from any user's device.

If you find a file in this folder that appears to contain real
material, please report it via `security@raven-messager.com`
immediately so we can rotate the affected material.

---

## How to use these vectors

Once populated, the workflow is:

1. Read a vector file. Decode the hex inputs into your
   implementation's native types.
2. Run your implementation with those inputs.
3. Compare your output, byte-for-byte, to the `expected` field.
4. If they match, your implementation is interoperable with
   Raven for that primitive.
5. If they do not match, please open a clearly-titled issue or
   email `security@raven-messager.com`. If the bug is in our
   vectors, we want to know.

A small helper script (`scripts/verify.py`, planned) will run
all vectors through a reference Python implementation as a
sanity check.

---

## Coverage goals

The first published batch will cover:

1. Pairing root from fixed `zX || zPQ || transcript_hash`.
2. ATSAM sub-key derivation from a known `K_root` for each of the
   labels published in the overview.
3. BBE slot derivation for a small grid of `(K_BBE, epoch, nonce)`
   triples.
4. Live-confirmation MAC for both initiator and responder roles
   (to confirm role binding catches reflection).
5. Routing tag derivation across several `(K_route, nonce)` pairs.
6. PV-RAW encryption + WC-MAC under fixed pads, including:
   - One vector that decrypts correctly with the intended pad.
   - One vector that fails authentication when a single
     ciphertext bit is flipped.
   - One vector that fails authentication when the AAD is
     altered.

We will only publish vectors that have been independently
recomputed by at least two implementations to avoid spec-vs-test
drift.

---

## Not yet published

- File round-trip examples (out of scope for v1).
- Performance benchmark vectors (different repository).
- Side-channel test inputs (only meaningful with a specific
  implementation).
