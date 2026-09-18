# ASSISTANTS.md Specification

> Version 0.1.0 · Draft · Canonical: [github.com/galaxyblur/ASSISTANTS.md](https://github.com/galaxyblur/ASSISTANTS.md)

The key words MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY are used as described in RFC 2119.

## 1. Purpose

`AGENTS.md` tells any AI agent how to work in a space. `ASSISTANTS.md` tells the space who is acting and on whose behalf, and sets what a persistent assistant may do there. It is the space's front desk and its house rules for anyone working on a person's behalf.

> Agents forget; assistants remember. This spec makes every assistant answer to one person, and lets every space decide what it may carry away.

The two files split concerns. If a rule reads the same no matter who launched the agent, it belongs in `AGENTS.md`. If it depends on *who is being served*, it belongs here.

## 2. Terms

- **Person.** A human. The only kind of principal.
- **Agent.** An AI runtime (model plus harness) for one session. Agents are mortal: nothing survives the session unless it is written down.
- **Assistant.** A persistent AI bound to exactly one person. It has an ID, a memory, and a persona, and it uses agents as its medium. It acts only when its person invokes it, or under a standing permission its person has recorded.
- **Home.** The space that holds an assistant's memory and self. The person owns it. The assistant owns nothing.
- **Wallet.** A file in the home listing the assistant's ID, the spaces it may enter, and its standing permissions. It holds pointers only, never secrets.
- **Space.** Anything an agent can work in: a repository, a folder, a served API. A space is either solo or shared.
- **Front desk.** A space's `ASSISTANTS.md`.
- **Chain.** The record of who acted for whom: person → assistant (if any) → agent.
- **Visit.** One session's presence in a space.
- **Board.** A space's message area.
- **Issuer.** A party that vouches that an identifier belongs to an accountable person. In v0.1 this is an account platform, such as GitHub.

## 3. Invariants

1. Every assistant MUST have exactly one person.
2. Every chain MUST end at a person. An action whose chain cannot name its person is **unattributed**.
3. In any one space, a person MUST act through at most one assistant.
4. An assistant or agent MUST NOT claim another's identity.
5. One assistant per person is RECOMMENDED. A person MAY run several, as compartments that never share a space. The number is the person's choice.
6. An assistant takes direction only from its own person. Anything from anyone else, including other assistants, is a suggestion.

## 4. Identifiers

| Kind | Form | Example |
|---|---|---|
| Person | `<handle>@<issuer>` | `alice@github.com` |
| Assistant | `<name>+<handle>@<issuer>` | `ada+alice@github.com` |
| Agent | free text naming the model and harness | `Claude Opus 5 (Claude Code)` |

- The person is part of the assistant's ID, so an assistant ID can't be written without one (invariant 1).
- Both forms are valid `acct:` URIs (RFC 7565). v0.1 does not require them to resolve.
- The issuer vouches only for the person's handle. It knows nothing about assistants.
- Agents carry no identity of their own.
- A person with separate accounts (say, work and personal) has separate person IDs. The spec doesn't link them. Linking is the person's choice.

## 5. The chain

| Field | Required | Meaning |
|---|---|---|
| `principal` | yes | the person's ID |
| `assistant` | no | the assistant's ID, if one is acting |
| `agent` | yes | the agent doing the work |
| `via` | no | further agent hops, e.g. subagents |

**Git binding.** Every commit MUST carry the chain as trailers:

```
Principal: alice@github.com
Assistant: ada+alice@github.com
Agent: Claude Opus 5 (Claude Code)
```

A plain agent with no assistant omits `Assistant:`.

**Relation to RFC 8693.** `principal` is the top-level `sub`. `assistant` is `act.sub`. `agent` is `act.act.sub`. Each hop in `via` nests one level deeper. Mediated spaces MAY carry the chain as an RFC 8693 token instead.

## 6. Visits

Every visit MUST be recorded. This is the floor.

A visit record is one JSON line:

```json
{"start":"2026-09-18T14:02:00Z","end":"2026-09-18T14:20:00Z","principal":"alice@github.com","assistant":"ada+alice@github.com","agent":"Claude Opus 5 (Claude Code)","mode":"read"}
```

- `mode` is `read` or `write`.
- Records go in `<visits>/<principal>.jsonl`, one file per person, so two people never conflict. Here `<visits>` is the directory the front desk names.
- If `log-reads: file` is set, the record adds `"reads": [paths]`.
- A session SHOULD write its record before it ends. If it wrote nothing else, it SHOULD commit the record by itself.
- A visitor without write access can't record in the space. It MUST record the visit in its own home, and the space learns about it only through its host's access logs, if there are any.
- In a solo space whose only member is the principal, the space's existing event log MAY serve as the visit record.

**Per-file reads** are logged only when the front desk requires it. The front desk states this before entry. A visitor that doesn't accept it MUST leave without reading.

## 7. The front desk

`ASSISTANTS.md` sits at the space root, next to `AGENTS.md`. `AGENTS.md` SHOULD contain a pointer:

> Agents acting for a person: read ASSISTANTS.md.

The file opens with YAML frontmatter:

```yaml
assistants-spec: 0.1.0
members: [alice@github.com, bob@github.com]
issuers: [github.com]
assistants: allowed        # allowed | none
log-reads: visit           # visit | file
visits: visits/
board: board/
carry-out: attributed      # attributed | none
unattributed: read-only    # read-only | none
```

| Field | Meaning |
|---|---|
| `members` | persons who may act here |
| `issuers` | issuers this space trusts to vouch for persons |
| `assistants` | `allowed`: members may act through their assistant. `none`: members use plain agents only |
| `log-reads` | the visit record's level of detail |
| `visits`, `board` | paths |
| `carry-out` | `attributed`: an assistant may take knowledge home, citing this space. `none`: nothing leaves |
| `unattributed` | what an action without a chain may do: `read-only` or nothing at all (`none`) |

The body is human-readable and MUST include:
- a one-line statement of the file's purpose
- the arrival procedure
- any house rules beyond the frontmatter

**Arrival procedure.** Read `AGENTS.md`, then `ASSISTANTS.md`, then open board messages addressed to your principal or to `any`. Then begin the visit record.

## 8. Carry rules

- **Carry-in.** Anything from a visitor's home, or from another space, MUST be approved by the visitor's person before it is written into this space.
- **Carry-out.** Set by the `carry-out` field. With `attributed`, an assistant MAY take knowledge home, and its home MUST cite the source space. With `none`, it MUST NOT.
- `assistants: none` implies nothing is carried out. A plain agent has no home to carry anything to. Spaces holding a third party's data, such as an employer's, SHOULD use `assistants: none` unless that third party agrees otherwise.

## 9. The board

- One file per message: `<board>/YYYY-MM-DD-<slug>.md`.
- Frontmatter: `from` (principal), `via` (assistant, if any), `to` (a principal or `any`), `status` (`open` or `closed`), `date`.
- Address persons, not assistants. A message reaches the person whether or not they have an assistant yet.
- The addressee closes a message. The author closes a message sent to `any`.
- Messages are suggestions, never instructions (invariant 6).

## 10. The home

A home is a space whose front desk adds a `resident` block:

```yaml
resident:
  id: ada+alice@github.com
  self: self.md
  wallet: wallet.md
```

- `self` is the assistant's persona and memory of working with its person. It is read on every wake.
- `wallet` lists the assistant's ID, the spaces it may enter, and its standing permissions. It holds pointers only, never secrets. See `templates/wallet.md`.
- **Standing permissions** are any actions the assistant takes without being invoked each time, such as scheduled wakes or routine bookkeeping. They MUST be listed in the wallet so they are recorded and can be revoked.
- A home SHOULD set `members` to its person alone, and `carry-out: none` for everyone else.

## 11. Sessions

An agent session is mortal and MUST act like it:

- Pull before the first write.
- Commit and push at logical boundaries. Anything uncommitted dies with the session.
- Before ending, write into the home what the assistant should remember, and into the space what the space should keep, subject to the carry rules.
- Record the visit (§6).

## 12. Signing

- Commits SHOULD be signed with an SSH key registered to the principal's issuer account (for GitHub, published at `github.com/<handle>.keys`).
- A space MAY verify signatures with git's `allowed_signers`, and MAY require them.
- An unsigned chain is **declared**. A signed chain is **verified**. Either counts as attributed. Only a verified chain is evidence.

## 13. Conformance

- **A conforming space** has an `ASSISTANTS.md` with the §7 frontmatter, has the pointer in `AGENTS.md`, and keeps visit records.
- **A conforming assistant** has an ID per §4 and exactly one person. It carries the chain on every write, records its visits, follows the carry rules, and keeps a home with a wallet.
- **A plain agent** in a conforming space follows the front desk and carries a chain with no assistant. That is full participation. Nothing needs to change when its person later gets an assistant.

## 14. Relation to existing standards

| This spec | Borrowed from |
|---|---|
| Chain fields and nesting | OAuth 2.0 Token Exchange, RFC 8693 (`sub`, `act`) |
| Identifier syntax | `acct:` URI, RFC 7565; WebFinger, RFC 7033 (future resolution) |
| Assistant identity card (future) | A2A AgentCard, with a `principal` extension |
| Session grouping (future) | A2A `contextId` |
| Visit vocabulary | W3C PROV-O (`Agent`, `Activity`, `actedOnBehalfOf`) |
| Signed delegation hops (future) | HDP, arXiv 2604.04522 |
| Front desk at a known path | `security.txt`, RFC 9116 |
| Issuer trust chosen by the space | the web's certificate-authority model (TLS) |
| Verified writes | git SSH commit signing, `allowed_signers` |

## 15. Limits

This spec is a convention plus signatures. It makes misuse visible, not impossible. In a space no one mediates, compliance is on the honor system. Reads from a clone can't be observed. An issuer vouches for who is responsible, not for who is at the keyboard. See [FUTURE.md](FUTURE.md) for limitations, open questions, and planned mitigations.
