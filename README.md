# ASSISTANTS.md

A front desk and house rules for AI assistants acting on a person's behalf.

`AGENTS.md` tells any AI agent how to work in a space: build steps, conventions. It assumes an anonymous tool that forgets everything when the session ends. That assumption is breaking. People now run assistants with memory, a name, and an existence beyond one session. These assistants move between spaces, carry knowledge with them, and act for someone.

`ASSISTANTS.md` sits next to `AGENTS.md`. `AGENTS.md` stays the document about interaction: what an agent may do in a space, and how. `ASSISTANTS.md` is about the exchange of information, and nothing else:

- Who is acting here, and for whom? Who owns this space?
- May a visitor that remembers come in?
- What may it bring in, and what may it carry away?
- How do spaces leave word for each other?

It never says what work an assistant may do. That is whatever `AGENTS.md` lets any agent do.

> Agents forget; assistants remember. This spec makes every assistant answer to one person, and lets every space decide what it may carry away.

New here? Read the [examples](EXAMPLES.md) first.

## Agent vs assistant

| | Agent | Assistant |
|---|---|---|
| Lifespan | one session | persistent; dormant between sessions |
| Memory | none | yes, kept in its person's home |
| Belongs to | whoever launched it | exactly one person, by design |

An assistant uses agents as its medium, the same way a person does.

## The rules

1. Every assistant has exactly one person.
2. Every action traces back to that person.
3. In any one space, a person acts through at most one assistant.
4. No assistant claims another's identity.
5. One assistant per person is recommended. Separate compartments are the person's choice.
6. An assistant takes direction only from its own person.
7. The person decides. An assistant holds memory for its person, never understanding.
8. Every space has exactly one owner, and the owner is a person.
9. An assistant has exactly one home, and a home houses one assistant.
10. A space appears in at most one of a person's wallets. Compartments don't bleed.

An assistant that isn't tied to exactly one person leaves no one responsible for it. A space that isn't tied to exactly one person has the same problem. These rules make accountability part of the design, not a matter of discipline.

## Idiocorpus, idiocortex, home

A person's own knowledge space is an **idiocorpus**: *idio-* (one's own, as in idiolect) plus *corpus* (a body of texts). Its purpose is knowledge, not a project with some other goal. When an assistant lives there it is an **idiocortex**, and it is that assistant's **home**. The assistant animates the corpus; it is not the corpus. It remembers for its person and does not understand for them.

A home is marked by files at its root, with fixed names so any agent can tell by looking:

| File | Holds | Travels? |
|---|---|---|
| `ASSISTANT_ID.md` | who the assistant is and whose | yes |
| `ASSISTANT_WALLET.md` | the spaces it may enter, its standing permissions | one entry at a time |
| `ASSISTANT_SELF.md` | its persona and its memory of working with its person | never |
| `ASSISTANT_SELF_PUBLIC.md` | optional: the part of self that may be seen elsewhere | yes |

`ls ASSISTANT*` shows everything this spec touches in a space.

## Adopt it

**In a space** (a repo, a shared folder):

1. Copy [`templates/ASSISTANTS.md`](templates/ASSISTANTS.md) to the root and fill in the owner and the members.
2. Add this line to `AGENTS.md`: `Agents acting for a person: read ASSISTANTS.md.`
3. Create `visits/` and `board/`. In a git repo you can set `visits: git` instead of keeping `visits/`: commits carrying the chain are the record. A space that wants neither sets `visits: none` or `board: none`; a visiting assistant then records its visit in its own home.

People without an assistant work there with a plain agent. That's full participation.

**For your assistant's home:**

1. Use [`templates/ASSISTANTS.home.md`](templates/ASSISTANTS.home.md) as the home's front desk.
2. Give the assistant an ID: `<name>+<your-handle>@<issuer>`, e.g. `ada+alice@github.com`.
3. At the root, add [`ASSISTANT_ID.md`](templates/ASSISTANT_ID.md), [`ASSISTANT_WALLET.md`](templates/ASSISTANT_WALLET.md) and [`ASSISTANT_SELF.md`](templates/ASSISTANT_SELF.md). Add [`ASSISTANT_SELF_PUBLIC.md`](templates/ASSISTANT_SELF_PUBLIC.md) if the assistant will work on machines that can't reach the home.
4. On each machine, install [`tools/assistants-visit`](tools/assistants-visit), list the home's local path in `~/.config/assistants/homes`, and run the tool from your harness's session-start hook. A session opened in any wallet space then wakes the assistant. Everywhere else it stays a plain agent.
5. On a machine that can't reach the home, run `assistants-visit --pack <space>` where the home is, and copy the directory it prints to the same path there. That carried set holds the ID, the public self, and the one wallet entry for that space.

**Adding a space through your assistant.** Once you have a home, give your assistant this prompt from the home:

```text
Make <repo path or URL> one of your spaces.

1. If it already has an ASSISTANTS.md, read it. If it allows assistants and
   lists me as a member, add it to your wallet and stop. Its house rules win.
2. Otherwise, take inventory of it before you change anything: agent guide,
   commit conventions, hooks, remote visibility, third-party data, and who
   else works there.
3. Ask me at most 3 questions: owner and members, whether assistants are
   allowed, and carry-out.
4. In that repo, add the front desk from the spec's templates/ASSISTANTS.md,
   pinned to the latest version. Add the pointer line to its AGENTS.md,
   creating the file if it's missing. Choose a visit record that fits the
   repo, and commit there with the chain.
5. At home, add the repo to your wallet with its role and log the adoption.
```

The prompt names neither the assistant nor the home, so it works unchanged for any assistant. Step 1 covers spaces you don't own: the space's front desk decides whether your assistant may enter, and the wallet only records that it can.

**Upgrading.** In any space you own, or in your home, tell your agent: *adopt the latest ASSISTANTS.md spec here.* [UPGRADING.md](UPGRADING.md) gives it the steps from each version to the next, the few questions it may ask, and the default for everything else. Only a space's owner changes its front desk; anyone else proposes through the board.

**Requiring a version.** A space can set `min-spec: 0.5.0` in its front desk. An assistant that follows an older spec stays out, and its person's session goes on as a plain agent. `assistants: none` refuses every assistant.

**Showing who's working.** `assistants-visit --id [dir]` prints the resident's ID when `dir` is its home or a wallet space, and nothing anywhere else. Anything that labels a session can use it. For example, a Claude Code status line badge (`ADA`):

```sh
id=$(~/.local/bin/assistants-visit --id "$(echo "$input" | jq -r .workspace.project_dir)")
[ -n "$id" ] && printf '%s  ' "$(echo "${id%%+*}" | tr a-z A-Z)"
```

Or a session-start hook that labels the pane `ada (claude)` in [herdr](https://herdr.dev):

```sh
id=$(~/.local/bin/assistants-visit --id "$CLAUDE_PROJECT_DIR") && [ -n "$id" ] && [ -n "$HERDR_PANE_ID" ] &&
  herdr pane report-metadata "$HERDR_PANE_ID" --source assistants --agent claude --display-agent "${id%%+*} (claude)"
```

Name the assistant and the agent together: the assistant persists, and the agent is replaceable.

## Files

- [SPEC.md](SPEC.md): the specification (v0.5.0, draft)
- [EXAMPLES.md](EXAMPLES.md): nine short stories of the spec in use. Start here
- [UPGRADING.md](UPGRADING.md): moving a space or a home to the latest version, step by step
- [FUTURE.md](FUTURE.md): open questions, limitations, ideas
- [templates/](templates/): front desks, the home files (ID, wallet, self, public self), board message
- [tools/](tools/): `assistants-visit`, which wakes an assistant in its wallet spaces, names it with `--id` for status lines and pane labels, and with `--pack` writes a carried set
- [CHANGELOG.md](CHANGELOG.md)

## Status

v0.5.0 is a draft, and this version is meant to be usable today with plain git and markdown. It borrows from OAuth token exchange (RFC 8693), A2A, W3C PROV, and git commit signing, and cites each of them in [SPEC.md §14](SPEC.md#14-relation-to-existing-standards).
