# Security Policy

## Reporting a Vulnerability

**Do not file security issues as public GitHub issues, Discord posts, or PRs.** This package implements the Signal Protocol — session, prekey, and identity-key handling. A public report is an exploit broadcast, and downstream projects (notably Baileys and other WhatsApp-protocol clients) consume `libsignal-node` directly, so a vulnerable version is in the wild within minutes of disclosure.

Use either of these private channels:

1. **Preferred:** [GitHub Security Advisories](https://github.com/WhiskeySockets/libsignal-node/security/advisories/new) on this repository. This gives us a private workspace to coordinate the fix, draft a CVE, and notify downstream projects.
2. **Email:** **rajeh@reforward.dev** — Rajeh Taher, current maintainer. Use this if GitHub Advisories is unavailable, or if the report involves the maintainer's keys / accounts directly.

If you don't get an acknowledgement within **72 hours**, please re-send — mail filtering does occasionally swallow security reports.

## What to include

A useful report contains:

- **Affected versions** (commit SHA or release tag)
- **Component**: session cipher, prekey/signed-prekey handling, sender keys, identity key store, curve25519 wrapper, fingerprint generation, etc.
- **Impact**: what an attacker can do (decrypt messages outside the intended audience, impersonate identities, replay, downgrade, key recovery, DoS of the protocol state) and under what preconditions
- **Reproduction**: minimal code or a deterministic test case. If the bug requires specific session state, include the construction steps — don't attach real session state from a production account.
- **Suggested fix** (optional but appreciated)

If a fix touches on-disk session state or key serialization formats, please flag that explicitly — those changes need extra coordination because they affect persisted state on every downstream user.

## What we treat as a vulnerability

In scope:

- Identity key, signed prekey, or one-time prekey leakage or downgrade
- Session, prekey, or sender-key handling that lets a remote party impersonate, replay, or decrypt outside the intended audience
- Cryptographic flaws: incorrect ratcheting, nonce reuse, MAC bypass, signature forgery, curve25519 misuse, weak randomness in key generation
- Memory corruption / RCE via crafted protocol messages or protobuf payloads
- Logic bugs that bypass intended access controls in the session / group session machinery
- Vulnerabilities in dependencies (`curve25519-js`, `protobufjs`) that are reachable through the public API
- Information disclosure of key material or plaintext through logs, error messages, or thrown exceptions on default configurations

Out of scope (please file as regular issues or PRs):

- Bugs that only affect availability of the local process (a malformed input that throws an exception you can catch is not a CVE; a malformed input that lets a remote party crash any libsignal-node consumer is)
- Issues in third-party forks or in higher-level libraries that wrap this one (report those to that project)
- Behavior of the Signal Protocol specification itself — we implement it; we don't define it
- Reports against the upstream `libsignal` (Rust/Java/Swift) implementation — this is a Node.js port; many upstream advisories will not be reachable here, and vice versa

## Coordinated disclosure

Our default timeline:

| Day | Step |
|---|---|
| 0 | Report received, acknowledged within 72h |
| ≤ 7 | Initial assessment shared with reporter; severity and rough timeline agreed |
| ≤ 30 | Fix prepared on a private branch; reporter invited to validate if they want |
| Coordinated date | Fix merged, release published, advisory + CVE published |

If you need to disclose sooner (e.g., active exploitation in the wild), tell us — we'll prioritize accordingly. If a fix is taking longer than 30 days, we'll keep you updated and explain why.

Because downstream consumers (Baileys, WhatsApp-protocol clients, other Signal-Protocol applications) cache session state across releases, we will coordinate the disclosure window with major downstream maintainers when the fix requires re-keying or migration.

We're happy to credit reporters in the advisory. Tell us how you'd like to be credited (name, handle, organization) or if you'd prefer to remain anonymous.

## Supported versions

Security fixes are applied to the **latest major release line**. Older majors are unmaintained — please upgrade. The `master` branch is also patched immediately, but downstream pinning to `master` is at your own risk for stability.

| Version | Supported |
|---|---|
| `2.x` (current) | ✅ |
| `< 2.0` | ❌ |

## Operational security for downstream users

A few notes for projects that consume `libsignal-node` — these aren't vulnerabilities in this library, but they are common ways production systems leak:

- **Identity keys are long-lived credentials.** The contents of any `SignalProtocolStore` you persist (identity keys, signed prekeys, sessions) are equivalent to an SSH private key for the account that owns them. Store them with the same care: not in container images, not in source control, encrypted at rest where you can.
- **Don't log session state or message plaintext.** It's easy to add a `console.log(decryptedMessage)` while debugging and never remove it. Filter aggressively in production.
- **Don't paste session state or store contents into AI tools, screenshots, or support tickets.** It's not redactable — anyone with the store contents can impersonate the account and decrypt messages.
- **Pin a release.** Pulling from `github:WhiskeySockets/libsignal-node` always grabs `master`. Use a tagged release in production so security advisories actually map to versions you ran.
