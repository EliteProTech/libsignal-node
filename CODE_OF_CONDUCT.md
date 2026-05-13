# Contributor Covenant Code of Conduct

## Our Pledge

We as members, contributors, and leaders pledge to make participation in the libsignal-node community a harassment-free experience for everyone, regardless of age, body size, visible or invisible disability, ethnicity, sex characteristics, gender identity and expression, level of experience, education, socio-economic status, nationality, personal appearance, race, religion, or sexual identity and orientation.

We pledge to act and interact in ways that contribute to an open, welcoming, diverse, inclusive, and healthy community.

## Our Standards

Examples of behavior that contributes to a positive environment:

- Demonstrating empathy and kindness toward other people
- Being respectful of differing opinions, viewpoints, and experiences
- Giving and gracefully accepting constructive feedback
- Accepting responsibility and apologizing to those affected by our mistakes, and learning from the experience
- Focusing on what is best for the overall community

Examples of unacceptable behavior:

- Sexualized language or imagery, or sexual attention or advances of any kind
- Trolling, insulting or derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information without explicit permission
- Other conduct which could reasonably be considered inappropriate in a professional setting

## Project-Specific Conduct

`libsignal-node` is a cryptographic library. The cost of a bad change here is high: it's not "a feature breaks for one user," it's "every downstream Signal-Protocol application weakens or breaks session state for every account they manage." Contributions are held to a corresponding standard.

Contributions that fall into the following categories will be rejected:

- **Weakening crypto.** Replacing primitives, lowering iteration counts, swapping a constant-time comparison for a non-constant-time one, removing MAC checks, accepting unauthenticated payloads, or "optimizing" away anything that looks like a defensive check. If the goal of a change is to make the protocol *do less*, it needs a security review, not a merge.
- **Adding backdoors, key escrow, or "debug" modes that bypass authentication.** This is not negotiable, including under the framing of "but it's only on by default in development."
- **Breaking on-disk session formats without a migration story.** Downstream users have persisted state. Changes to serialization need explicit handling, not silent breakage.
- **Bypassing the Signal Protocol's intended properties** (forward secrecy, post-compromise security, deniability) for convenience — e.g., disabling ratchet stepping, persisting old chain keys, or logging plaintext into the store.

Discussion of these areas — *understanding* why a check exists, *proposing* a safer alternative, *fixing* a bug that happens to touch them — is welcome and necessary. The line is intent.

## AI Policy

This project welcomes AI-assisted contributions. Most of us use them. For a cryptographic library, the rules are stricter than usual:

1. **You are the author.** AI tools (Claude, Copilot, Cursor, Codex, ChatGPT, etc.) are tools. The human who opens the PR is responsible for every line in it — correctness, license compatibility, security implications, and adherence to the rest of this Code of Conduct. "The AI wrote it" is not a defense, and it's an especially weak one for crypto code.

2. **Disclose AI use in PRs.** A one-line note in the PR description is enough — for example: *"Drafted with Claude Code, reviewed and tested by me."* You don't need to enumerate every prompt. The point is that reviewers know to look closer at things AI tools commonly get wrong (hallucinated APIs, invented protocol details, plausible-but-wrong refactors).

3. **Review before you submit.** Don't open PRs with code you haven't read. Don't open PRs with code you don't understand well enough to defend in review. If a reviewer asks "why does this work?" and your answer is "the AI suggested it," the PR will be closed.

4. **Test what the AI produced.** Run the test suite locally. AI tools confidently generate code that doesn't compile, doesn't pass existing tests, or — worse for this project — passes tests but quietly breaks an invariant the tests don't cover.

5. **Don't paste secrets into AI tools.** This includes:
   - Identity keys, signed prekeys, one-time prekeys, sender keys, sessions — any contents of a real `SignalProtocolStore`
   - `.env` files, API tokens, signing keys
   - Real user phone numbers, message content, or contact lists from production systems
   Most AI providers retain prompts for some period; treat anything you paste as published.

6. **Don't use AI to mass-generate issues, PRs, or comments.** Drive-by AI-generated PRs that "fix" non-bugs, add unnecessary tests, or reformat unrelated code waste maintainer time and will be closed without detailed review. If you're using an autonomous agent, supervise it — especially anywhere near crypto code.

7. **Hallucinated protocol details are a security issue.** The Signal Protocol has a specific definition. AI tools will confidently invent ratchet behavior, key-derivation orderings, message-keys structures, and serialization layouts that look plausible and are wrong. Verify against the upstream Signal specification, the existing test vectors, or a known-good implementation before submitting anything that touches the protocol state machine.

8. **License-compatible output only.** If you use an AI tool that may surface code from incompatible licenses (proprietary, or licenses incompatible with GPL-3.0), don't submit that output. We can't accept it. `libsignal-node` is GPL-3.0.

Following this policy doesn't slow you down — it just keeps the bar consistent between AI-assisted and hand-written contributions.

## Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be reported to the maintainers at **rajeh@reforward.dev**. All complaints will be reviewed and investigated promptly and fairly. The maintainers are obligated to respect the privacy and security of the reporter of any incident.

For security vulnerabilities (which are not Code of Conduct issues), see **[SECURITY.md](SECURITY.md)**.

## Enforcement Guidelines

Maintainers will follow these Community Impact Guidelines in determining the consequences for any action they deem in violation of this Code of Conduct:

### 1. Correction

**Community Impact:** Use of inappropriate language or other behavior deemed unprofessional or unwelcome in the community.

**Consequence:** A private, written warning from the maintainers, providing clarity around the nature of the violation and an explanation of why the behavior was inappropriate. A public apology may be requested.

### 2. Warning

**Community Impact:** A violation through a single incident or series of actions.

**Consequence:** A warning with consequences for continued behavior. No interaction with the people involved, including unsolicited interaction with those enforcing the Code of Conduct, for a specified period of time. Violating these terms may lead to a temporary or permanent ban.

### 3. Temporary Ban

**Community Impact:** A serious violation of community standards, including sustained inappropriate behavior.

**Consequence:** A temporary ban from any sort of interaction or public communication with the community for a specified period of time. Violating these terms may lead to a permanent ban.

### 4. Permanent Ban

**Community Impact:** Demonstrating a pattern of violation of community standards, including sustained inappropriate behavior, harassment of an individual, or aggression toward or disparagement of classes of individuals. Also: contributions or behavior aimed at weakening the cryptographic guarantees of the library (see § Project-Specific Conduct).

**Consequence:** A permanent ban from any sort of public interaction within the community.

## Attribution

This Code of Conduct is adapted from the [Contributor Covenant](https://www.contributor-covenant.org/), version 2.1, with project-specific and AI-policy sections added by the maintainers of `libsignal-node`.
