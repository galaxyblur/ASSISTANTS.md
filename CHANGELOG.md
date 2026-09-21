# Changelog

Follows [Semantic Versioning](https://semver.org/).

## Unreleased

**The spec is now a framework.** `SPEC.md` is one page: six words, eleven principles, what a space states, and the limits. It names no file formats and no tools. Everything concrete (identifiers, the chain's git trailers, visit records, front desk fields, the home files, the carried set, signing, conformance) moved unchanged to `BINDING.md`, the reference binding for git and markdown, with its section numbers kept. Where the two disagree, the framework wins. **Removed: the understanding check** (0.4.0's invariant 7 and §11 *Before a decision*). Briefing a person and checking their understanding before they decide is one person's practice with their own assistant. It belongs in that person's `ASSISTANT_SELF.md` and was never a rule about information crossing a boundary. The binding keeps the invariant's number, empty. **The subject is records, not assistants.** The framework is about who is acting in a space, what may come in, and what may be written down elsewhere afterwards. It applies to any visitor: a person by hand, an agent, an assistant. To leave a space is to be written down anywhere outside it, a tool's memory included, so `carry-out` binds a plain agent whose harness remembers. Seven principles are general and four are for assistants. Taken out of the framework: the agent and chain as words, "whether assistants may enter" as something a space states, and "two rules in conflict means stop and ask". The binding still has all three.

**Identity** is a new word: the account that names a person, and all a space ever sees. An assistant is bound to one identity and an identity has at most one assistant, so a member-list line admits exactly one assistant and compartments are separate identities. This closes a hole where two assistants of one person could both enter a space as that person. The binding's invariants 3, 5 and 10 still describe the older model. Principle 3 now says outright that a policy is the owner's to make exceptions to.

**Carry-out has no assumed destination.** It was defined as space → home, while §8 also used it to gate board messages between spaces, §10 told homes to set `none`, and Example 4 sent a message out of a `none` space. Now: carry-out is information leaving a space with an assistant, wherever it lands, under one rule. `attributed`: it may leave, citing the space, at any destination. `none`: nothing leaves on the assistant's initiative; the space's owner may release one item at a time as a board message, which is the record. A home is the same case, its person being its owner. Example 4 and the templates follow.

## 0.5.0 (2026-09-20)

**Scope.** `ASSISTANTS.md` is about the exchange of information; `AGENTS.md` remains the document about interaction (§1). The spec covers four boundaries: what comes into a space, what leaves it, what is recorded about who was there and for whom, and what passes between an assistant and its home. It no longer says what an assistant may do in a space or how. Cut or made non-normative on that test: §11's git cadence (pull, commit, push), and the knowledge-layer rule for sessions visiting from the home. `unattributed` is reworded as an information rule with the same behavior.

**Owner** (new invariant 8, new `owner` field): every space has exactly one owner, a person, inside an organization too. A pre-0.5 front desk with one member: that member is the owner.

**Idiocorpus and idiocortex** (§2, §10): a person's own knowledge space is an idiocorpus; with a resident assistant it is an idiocortex, and "home" is the role it plays. One assistant per home and one home per assistant (invariant 9). A space appears in at most one of a person's wallets (invariant 10).

**Breaking: the home files.** A home is marked by three fixed files at its root, replacing the front desk's `resident` block: `ASSISTANT_ID.md` (small, safe to show), `ASSISTANT_WALLET.md`, `ASSISTANT_SELF.md`. ID and self are now separate. Optional `ASSISTANT_SELF_PUBLIC.md` is the person-approved extract of self that may be seen outside the home. `tools/assistants-visit` reads the new files and still reads a `resident` block when they are absent, until 0.6. To migrate: move the self and wallet pages to the root names, add `ASSISTANT_ID.md`, delete `resident`.

**The carried set** (§8, §11): the home must be reachable, or the assistant brings a dated read-only snapshot: its ID, its public self, and the one wallet entry for the space being visited. Never the full self or the full wallet. `assistants-visit --pack` writes one. With neither home nor carried set, the session is a plain agent.

**Policy is the space's:** `visits: none` and `board: none` are allowed. The assistant still records every visit, at home when the space keeps no record (§6). A space with no front desk is treated as `carry-out: none` with no board (§7). Board messages flow between spaces in both directions: the sender's person approves sending, and the receiving owner's policy decides acceptance (§8). §9 now uses RFC 2119 keywords.

**Versions and upgrading** (§7, new `UPGRADING.md`): a front desk may set `min-spec`; an assistant whose `ASSISTANT_ID.md` declares an older `assistants-spec` stays out and the session goes on as a plain agent. `assistants-visit` enforces it at the door. A visitor reads an older front desk's missing fields by stated defaults, and a newer one's unknown fields carefully. Only the owner changes a front desk. `UPGRADING.md` gives agent-followable steps between versions behind one instruction: *adopt the latest ASSISTANTS.md spec here.* New `EXAMPLES.md`: nine user stories.

Wallet: standing permissions take a `scope` (`home`, `all`, or a repo), which also decides what travels in a carried entry; `principal` moves to the ID file. New templates for the ID, self and public self. README lists all ten rules (it had omitted 7).

## 0.4.1 (2026-09-20)

A space receives through its board (§8, §9): to move something between a person's own spaces, the assistant writes a message to the receiving space's board, with approval and within the sender's `carry-out`. A session does not reach into another space to fetch, and the home is no exception beyond `self` and `wallet`. Follows from 0.4.0's closed home: a space that can't read the home still needs a way to be told things.

## 0.4.0 (2026-09-20)

The person decides (new invariant 7): an assistant can hold memory for its person but not understanding, so before a decision it SHOULD brief the state and check understanding with specific questions (§11). Only decisions are gated, the person may waive, and the brief follows the person's recorded preference, which now lives in `self` (§10). The home stays home (§8): a visiting assistant reads only its `self` and `wallet` from home and MUST NOT raise home matters in a space unless asked. `self` holds how to work with the person, never what is going on at home, and standing requests tied to home matters are scoped to home sessions. `tools/assistants-visit` now says so in its wake lines. Visiting from the home (§11): a home session that walks into a space keeps its identity and takes on the space's conventions, reads the space's `AGENTS.md` and `ASSISTANTS.md` in full before writing, stays in the space's knowledge layer, and hands anything else to a session started in the space; a conflict between home and space rules stops the work until the person rules. Found in use: a standing reminder written into `self` without a scope fired inside an unrelated space on the day it was written.

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
