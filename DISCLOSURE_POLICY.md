# Responsible Disclosure Policy

We appreciate security researchers who help make Raven safer.

## How to report

Email **`security@raven-messager.com`** with:

- A clear description of the issue.
- Steps to reproduce.
- Potential impact, ideally tied to a specific claim in
  [`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md).
- Suggested mitigation, if known.

For sensitive reports, you may PGP-encrypt your message. A PGP key
will be published here once we have one ready; in the meantime, send
us a brief notice and we can arrange a secure channel.

## What to expect from us

- **Acknowledgement within 7 days** of your report.
- Status updates as the investigation progresses.
- A coordinated disclosure timeline that prioritises user safety
  over PR.
- Public credit in release notes, with your permission.

## What we ask of you

- Give us reasonable time to investigate and remediate before any
  public disclosure. The exact window depends on severity; we will
  communicate a date.
- Do not access, modify, or destroy data belonging to users you do
  not control.
- Do not perform denial-of-service testing against production
  infrastructure.
- Do not exploit the issue beyond what is necessary to prove it.
- Do not publish exploit details before coordination.

## Safe-harbour intent

We will not pursue legal action against researchers who:

- Make a good-faith effort to follow this policy.
- Avoid privacy violations and service disruption.
- Do not extort, exfiltrate, or sell vulnerability details.
- Coordinate with us before public disclosure.

This intent does not waive any third-party rights (for example, app
store policies, OS vendor policies, or laws of jurisdictions
outside our control). When in doubt, ask us first via the security
email above.

## Scope

In scope:

- The ATSAM protocol documentation in this repository.
- Test vectors published here (when present).
- Open-source reference implementations published by Raven (when
  present).
- The Raven website at `https://raven-messager.com` (informational
  bugs only).

Out of scope:

- Spam, abuse, or content moderation. Use the in-app reporting flow.
- Social engineering of Raven employees or users.
- Physical access attacks against unlocked devices.
- Production infrastructure that requires credentials you should
  not have.

## What is not a vulnerability

To save researchers time, the following are not considered
vulnerabilities and have already been acknowledged in our public
documentation:

- "Raven leaks message timing and traffic volume."
  → Already acknowledged. See [`THREAT_MODEL.md`](THREAT_MODEL.md)
    section on global traffic analysis.
- "A relay can see that two devices are talking to each other."
  → Already acknowledged. Per-message routing tags reduce
    *linkability* but a single observed envelope still proves *some*
    pair is communicating.
- "Vault Mode is not safe for media."
  → By design. Vault Mode is text-only.
- "A compromised endpoint can leak everything."
  → By design. Endpoint security is out of ATSAM's threat model.

## After a fix ships

We aim to:

- Publish a brief, factual advisory describing the issue, impact, and
  mitigation.
- Credit the researcher (with permission).
- Update [`SECURITY_CLAIMS.md`](SECURITY_CLAIMS.md) if the issue
  revealed a gap between claim and reality.
