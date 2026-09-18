# Changelog

Follows [Semantic Versioning](https://semver.org/).

## 0.3.1 (2026-09-18)

`tools/assistants-visit --id [dir]` prints the resident's ID when `dir` is its home or a wallet space, and nothing elsewhere. README: showing who's working, with a status line badge and a herdr pane label as examples. No normative changes.

## 0.3.0 (2026-09-18)

Git history as the visit record (§6): a git space may set `visits: git`. Commits carrying the chain record write visits. A visit that commits nothing makes one empty commit carrying the chain. Not combinable with `log-reads: file`. The front desk table (§7) now splits `visits` from `board`. Found in the first two code-space adoptions (Daybreaker, dotfiles), where a per-visit record file only duplicated the commits.

## 0.2.1 (2026-09-18)

README: a standard prompt for adding a space through your assistant. You give it from the home, and it covers spaces that already have a front desk. No normative changes.

## 0.2.0 (2026-09-18)

Waking in a space (§11): an assistant whose session starts inside a space MUST wake from its home first, and where the home lives is the person's per-machine configuration. The arrival procedure (§7) now starts there. The wallet's `spaces` entries (`repo`, `role`) are named as the fields tools read (§10). New reference tool `tools/assistants-visit` for git spaces, run from a session-start hook.

## 0.1.1 (2026-09-18)

Third-party data: SHOULD `carry-out: none`, no longer SHOULD `assistants: none`. Retention was the concern, and carry-out already governs it. `assistants: none` is now for owners who refuse assistants outright.

## 0.1.0 (2026-09-18)

Initial draft. Terms, six invariants, identifiers, the chain and its git binding, visit records (visit-level floor, per-file reads only when the owner requires them), the front desk file, carry rules, board, home and wallet, session discipline, optional SSH signing, conformance, relation to existing standards.
