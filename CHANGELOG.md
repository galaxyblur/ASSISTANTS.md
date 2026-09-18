# Changelog

Follows [Semantic Versioning](https://semver.org/).

## 0.2.0 (2026-09-18)

Waking in a space (§11): an assistant whose session starts inside a space MUST wake from its home first, and where the home lives is the person's per-machine configuration. The arrival procedure (§7) now starts there. The wallet's `spaces` entries (`repo`, `role`) are named as the fields tools read (§10). New reference tool `tools/assistants-visit` for git spaces, run from a session-start hook.

## 0.1.1 (2026-09-18)

Third-party data: SHOULD `carry-out: none`, no longer SHOULD `assistants: none`. Retention was the concern, and carry-out already governs it. `assistants: none` is now for owners who refuse assistants outright.

## 0.1.0 (2026-09-18)

Initial draft. Terms, six invariants, identifiers, the chain and its git binding, visit records (visit-level floor, per-file reads only when the owner requires them), the front desk file, carry rules, board, home and wallet, session discipline, optional SSH signing, conformance, relation to existing standards.
