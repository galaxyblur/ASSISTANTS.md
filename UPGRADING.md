# Upgrading

How to move a space, or an assistant's home, to the latest spec. It is written so that an agent can follow it. The instruction a person gives is one line:

> Adopt the latest VISITORS.md spec here.

## The procedure

An agent given that instruction, in any space:

1. **Find where you are.** Read the `visitors-spec` pin in this space's `VISITORS.md`. Before 0.6 the file is `ASSISTANTS.md` and the key is `assistants-spec`. If there is no front desk, this is an adoption and not an upgrade: follow "Adopt it" in the [README](README.md).
2. **Find where the spec is.** Read the version line at the top of the canonical [GIT.md](https://github.com/galaxyblur/VISITORS.md/blob/main/GIT.md), and this file beside it.
3. **Check who is asking.** Only the owner changes a front desk (GIT.md §7). If the front desk names an `owner` and your principal isn't that identity, stop. Write the proposal to the space's board, addressed to the owner. You must also be a worker: a session started in this space. From anywhere else, the board is all you may write.
4. **Walk the steps.** Apply each section below in order, from the pin you found up to the latest. Skip none: each assumes the one before it.
5. **Ask only what the steps tell you to ask.** Everything else has a stated default. Ask all the questions at once, before changing anything.
6. **Change only what this spec owns.** The front desk, the `ASSISTANT_*` files, the board and visit directories, and the one pointer line in `AGENTS.md`. Leave the rest of `AGENTS.md` and the space alone.
7. **Bump the pin last,** to the version you reached. Save the change the way the space's `AGENTS.md` says work is saved, carrying the chain.
8. **Report** what changed, what you asked, and anything the steps left for the person to do by hand.

If the space is a home, do the home steps too. A home is a space whose front desk has a `resident` block (before 0.5) or that has `ASSISTANT_ID.md` at its root (0.5 on).

An upgrade never changes a policy on its own. Where a new field appears, its default below is the one that keeps the space behaving as it did.

## Defaults for fields a front desk lacks

A visitor meeting an older front desk reads a missing field as:

| Field | Since | Read as |
|---|---|---|
| `owner` | 0.5.0 | the sole member. With several members: no declared owner; tell your person |
| `min-spec` | 0.5.0 | no restriction |
| `visits: none`, `board: none` | 0.5.0 | not available below 0.5; a missing `visits` or `board` directory means the space hasn't made one yet |
| `visitors` | 0.6.0 | `unattributed: read-only` reads as `any`; otherwise `none` |
| `visit-log` | 0.6.0 | `log-reads` if set; `none` if `visits: none`; else `visit` |
| `carry-out: open`, `with-attribution` | 0.6.0 | `attributed` reads as `with-attribution` |

## 0.5.x → 0.6.0

**Every space**

1. Rename `ASSISTANTS.md` to `VISITORS.md`. In its frontmatter, rename `assistants-spec` to `visitors-spec`.
2. Move the fields to their 0.6 names. None of these changes the policy:
   - `log-reads: X` → `visit-log: X`. `visits: none` → `visit-log: none`, and drop `visits`.
   - `carry-out: attributed` → `carry-out: with-attribution`.
   - `unattributed: read-only` → `visitors: any`. `unattributed: none` → delete it; `visitors` defaults to `none`.
   - `members` is unchanged: it was always who may work.
3. Change the pointer line in `AGENTS.md` to: `Whoever works here for a person: read VISITORS.md.`
4. Fix anything else that names the old file: links, scripts, hooks.
5. Say in the report that a session started elsewhere is now a visitor here: it reads and writes the board, nothing else. Any house rule that let such a session edit part of the space is now void; point it out, and leave `AGENTS.md` for the owner.

**A home, also**

1. In `ASSISTANT_ID.md`, rename `assistants-spec` to `visitors-spec` and set it to `0.6.0`. The `ASSISTANT_*` files keep their names.
2. In the home's front desk, add `visitors: none` unless the person wants otherwise.
3. On each machine, install the 0.6 `tools/assistants-visit`. It still reads a space or a home that hasn't been renamed yet.

## 0.4.x → 0.5.0

**Every space**

1. Add `owner:`. Ask: *who is the one person accountable for this space?* If `members` lists one person, propose that person and ask only for a yes.
2. Nothing else is required. `visits` and `board` keep their values. `unattributed` keeps its value; only its wording changed.
3. Optional, and only if the owner asks for it: `min-spec: 0.5.0`, to admit only assistants that follow the closed home and the carried set.
4. In the body, the file's purpose line should now say what the file is about: what information may enter, what may leave, what is recorded. Rules about how work is done (pull, commit, push) belong in `AGENTS.md`; move any you find there, and say so in the report.

**A home, also**

1. Create `ASSISTANT_ID.md` at the root from [the template](templates/ASSISTANT_ID.md): `id` from the `resident` block, `principal`, `issuer`, `name`, and `visitors-spec: 0.5.0`.
2. Move the page `resident.wallet` names to `ASSISTANT_WALLET.md` at the root. Drop its `principal` key (it lives in the ID file now). Add a `scope` to each standing permission. Ask: *for each of these, does it apply at home only, everywhere, or in one space?* Default if the person doesn't care to answer: `home`, the careful reading.
3. Move the page `resident.self` names to `ASSISTANT_SELF.md` at the root.
4. Fix every reference to the two old paths: links in the home's pages, its `AGENTS.md`, scripts, launchers, harness settings. How links resolve is the home's own convention; check before assuming an alias or redirect will do.
5. Delete the `resident` block.
6. Optional: draft `ASSISTANT_SELF_PUBLIC.md` from the self page and show it to the person. It is not added until they approve its text.
7. On each machine: install the 0.5 `tools/assistants-visit`. The old one reads only the `resident` block and will stop waking the assistant once step 5 is done. Anything else that parsed the block needs the same fix. Do this before step 5 if the person can't tolerate a gap.

## 0.3.x → 0.4.x

No file changes in a space. In a home, read the self page against two rules and fix what fails: it holds *how* to work with the person and never *what* is going on at home, and any standing request tied to a home matter says it applies to home sessions only.

## 0.2.x → 0.3.x

Optional: a git space may replace its `visits/` directory with `visits: git`.

## 0.1.x → 0.2.x

No file changes in a space. In a home, the wallet's `spaces` entries need `repo` and `role` keys, which tools read. On each machine, install `tools/assistants-visit` and list the home in `~/.config/assistants/homes`.
