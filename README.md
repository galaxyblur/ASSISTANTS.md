# ASSISTANTS.md

A front desk and house rules for AI assistants acting on a person's behalf.

`AGENTS.md` tells any AI agent how to work in a space: build steps, conventions. It assumes an anonymous tool that forgets everything when the session ends. That assumption is breaking. People now run assistants with memory, a name, and an existence beyond one session. These assistants move between spaces, carry knowledge with them, and act for someone.

`ASSISTANTS.md` sits next to `AGENTS.md` and answers the questions `AGENTS.md` can't:

- Who is acting here, and for whom?
- Which assistants may enter?
- What may they carry away?
- How do they leave word for each other?

> Agents forget; assistants remember. This spec makes every assistant answer to one person, and lets every space decide what it may carry away.

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

An assistant that isn't tied to exactly one person leaves no one responsible for it. These rules make accountability part of the design, not a matter of discipline.

## Adopt it

**In a space** (a repo, a shared folder):

1. Copy [`templates/ASSISTANTS.md`](templates/ASSISTANTS.md) to the root and fill in the members.
2. Add this line to `AGENTS.md`: `Agents acting for a person: read ASSISTANTS.md.`
3. Create `visits/` and `board/`. In a git repo you can set `visits: git` instead of keeping `visits/`: commits carrying the chain are the record.

People without an assistant work there with a plain agent. That's full participation.

**For your assistant's home:**

1. Use [`templates/ASSISTANTS.home.md`](templates/ASSISTANTS.home.md) as the home's front desk.
2. Add a [`wallet.md`](templates/wallet.md) and a self page.
3. Give the assistant an ID: `<name>+<your-handle>@<issuer>`, e.g. `ada+alice@github.com`.
4. On each machine, install [`tools/assistants-visit`](tools/assistants-visit), list the home's local path in `~/.config/assistants/homes`, and run the tool from your harness's session-start hook. A session opened in any wallet space then wakes the assistant. Everywhere else it stays a plain agent.

**Adding a space through your assistant.** Once you have a home, give your assistant this prompt from the home:

```text
Make <repo path or URL> one of your spaces.

1. If it already has an ASSISTANTS.md, read it. If it allows assistants and
   lists me as a member, add it to your wallet and stop. Its house rules win.
2. Otherwise, take inventory of it before you change anything: agent guide,
   commit conventions, hooks, remote visibility, third-party data, and who
   else works there.
3. Ask me at most 3 questions: members, whether assistants are allowed, and
   carry-out.
4. In that repo, add the front desk from the spec's templates/ASSISTANTS.md,
   pinned to the latest version. Add the pointer line to its AGENTS.md,
   creating the file if it's missing. Choose a visit record that fits the
   repo, and commit there with the chain.
5. At home, add the repo to your wallet with its role and log the adoption.
```

The prompt names neither the assistant nor the home, so it works unchanged for any assistant. Step 1 covers spaces you don't own: the space's front desk decides whether your assistant may enter, and the wallet only records that it can.

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

- [SPEC.md](SPEC.md): the specification (v0.4.0, draft)
- [FUTURE.md](FUTURE.md): open questions, limitations, ideas
- [templates/](templates/): front desks, wallet, board message
- [tools/](tools/): `assistants-visit`, which wakes an assistant in its wallet spaces and, with `--id`, names it for status lines and pane labels
- [CHANGELOG.md](CHANGELOG.md)

## Status

v0.4.0 is a draft, and this version is meant to be usable today with plain git and markdown. It borrows from OAuth token exchange (RFC 8693), A2A, W3C PROV, and git commit signing, and cites each of them in [SPEC.md §14](SPEC.md#14-relation-to-existing-standards).
