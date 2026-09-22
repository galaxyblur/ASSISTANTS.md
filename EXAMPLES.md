# Examples

Short stories showing the spec in use. Alice and Bob are people. Ada is Alice's assistant. None of this is normative. [SPEC.md](SPEC.md) is the framework, and [GIT.md](GIT.md) is the git and markdown detail these stories use.

Each story says what someone wanted, what they set up, and what then happens.

---

## 1. Alice's notes get a resident

**Wants:** Alice keeps years of notes in a folder. She wants an assistant that remembers how she works and what it has told her before.

**Has:** a folder of notes. Her own space, nobody living in it.

**Does:** adds a front desk from [the home template](templates/VISITORS.home.md), then three files at the root:

```
notes/
├── AGENTS.md             how the notes are kept (hers, not this spec's)
├── VISITORS.md           owner: alice · members: [alice] · carry-out: none
├── ASSISTANT_ID.md       ada+alice@github.com
├── ASSISTANT_SELF.md     empty headings, to be filled by use
└── ASSISTANT_WALLET.md   one space: this one, role: home
```

**Then:** the folder is Ada's home. Any agent that opens it reads the ID file and knows whose assistant it is speaking as. Next month Alice swaps her agent for another vendor's. Ada is unchanged: same ID, same self, same record.

---

## 2. Ada works in a project

**Wants:** Alice has a code repo, `garden`. She wants Ada there too, without her notes following along.

**Does:** from her home, gives Ada the standard prompt (*Make `garden` one of your spaces*, in the [README](README.md)). Ada adds a front desk to `garden` with `owner: alice@github.com`, `visits: git`, and adds the repo to her wallet.

**Then:** Alice opens a session in `garden`. That makes it a worker: started in the space, as an identity that may work there. The session-start hook runs `assistants-visit`, which finds `garden` in Ada's wallet and prints the wake lines. The agent reads Ada's ID, self and wallet from the home and **nothing else**, then `garden`'s `AGENTS.md` and front desk.

What Ada may *do* in `garden` is whatever `AGENTS.md` lets any agent do. The front desk added nothing to that and took nothing away. It settled only three things: commits carry `Assistant: ada+alice@github.com`, those commits are the visit record, and what Ada learns there may leave with her, to her home or to another of Alice's spaces, if it cites `garden`.

Alice's notes never come up. Ada doesn't mention the dentist.

---

## 3. A repo shared with Bob

**Wants:** Alice and Bob work in one repo. Bob has no assistant and doesn't want one.

**Front desk:**

```yaml
owner: alice@github.com
members: [alice@github.com, bob@github.com]
assistants: allowed
board: board/
carry-out: with-attribution
```

**Then:** Bob works with a plain agent. His commits carry `Principal: bob@github.com` and an `Agent:` line, and no `Assistant:`. That is full participation.

Ada wants Bob to look at a draft. She writes `board/2026-09-18-q4-plan.md`, `from: alice`, `via: ada+alice`, `to: bob`. It is addressed to Bob's identity, so it reaches him whatever tools he uses. His agent shows it to him on arrival. It is a suggestion. Nothing obliges Bob or his agent to act on it, and Ada gives Bob's agent no instructions, ever.

Bob thinks the front desk should log file reads. He isn't the owner, so he doesn't edit it. He leaves a message on the board for Alice.

---

## 4. Client records

**Wants:** Alice does bookkeeping for clients. She wants Ada's help in that repo. The records belong to her clients, not to her.

**Front desk:** `carry-out: none`.

**Then:** Ada works there like anywhere else. When the session ends, nothing about the clients leaves: not to the home, and not to any other space. Next week Ada knows the space exists (the wallet lists it) and knows nothing of what is in it until she is inside again. Her home records only that a visit happened, and when.

One day Ada notices a better way to structure Alice's own notes, while working there. Under `carry-out: none` Ada can't take that out herself, whatever it is about. She tells Alice, in the session. Alice owns this space, so the release is hers to make: she reads the text, sees it says nothing about a client, and approves that one item by name. Ada posts it to the **home's** board, and a home session picks it up. The message is the record that the owner released one item. `none` means nothing leaves on Ada's say-so. It never meant the owner can't open her own door.

In a repo Alice didn't own, the same idea would go to the owner's board first, and leave only if the owner said yes.

---

## 5. Work and personal

**Wants:** Alice's employer lets staff use assistants in company repos. Alice wants help at work. She does not want her personal life at work, or the company's code in her notes.

**Does:** sets up a second home on her work account, with its own assistant: `ada+alice@corp.example`. Different identity, different home, different wallet.

**Then:** the two never meet. No space is in both wallets, and no board message crosses. The spec calls these compartments. It doesn't link the two Alices; that would be her choice, and she doesn't make it.

In each company repo, one named engineer is the `owner`, even though the company holds the repo. When that engineer leaves, the front desk has a line someone has to change. That is the point of the line.

---

## 6. A machine that can't see home

**Wants:** Alice runs agents on a cloud box that has `garden` checked out. Her notes aren't there and shouldn't be.

**Does:** at home, runs `assistants-visit --pack ~/code/garden` and copies the directory it prints to the cloud box. The carried set holds:

```
ASSISTANT_ID.md                 who Ada is, and whose
ASSISTANT_SELF_PUBLIC.md        how to speak and brief; approved by Alice
spaces/github.com_alice_garden.md   the one wallet entry, and the permissions scoped to it
PACKED                          when
```

**Then:** a session on the cloud box wakes as Ada, a little plainer than at home. It has her voice and her rules for briefing Alice. It does not have her record, Alice's other spaces, or anything from the notes. It can't leak what it never had. What it learns goes home as a board message.

With no carried set either, the session is a plain agent. It says so and carries on.

---

## 7. A space with a minimum

**Wants:** Bob runs a shared repo and relies on the closed home: he doesn't want a visiting assistant chatting about its owner's other projects in his repo's sessions. That rule exists from 0.4, and the carried set from 0.5.

**Front desk:** `min-spec: 0.5.0`.

**Then:** Carol's assistant follows 0.3. Its `ASSISTANT_ID.md` says so. At Bob's door, `assistants-visit` prints why it is staying out, and Carol's session goes on as a plain agent: full participation, nothing carried away. Carol is told that one instruction at her home would fix it.

Bob can also set `assistants: none`. That refuses every session that remembers, at any version.

---

## 8. Upgrading

**Wants:** `garden` is pinned to 0.4.1. Alice wants it current.

**Does:** in `garden`, says: *adopt the latest VISITORS.md spec here.*

**Then:** the agent reads the pin, reads [UPGRADING.md](UPGRADING.md), and checks that Alice owns the space. It asks her one question (*you're the only member; are you the owner?*), adds `owner:`, renames the front desk to `VISITORS.md`, moves the fields to their 0.6 names, bumps the pin, and saves the change the way `garden`'s `AGENTS.md` says to. It reports that the policy itself is unchanged.

At home the same instruction does more: it creates `ASSISTANT_ID.md`, moves the wallet and self pages to their root names, asks Alice to scope each standing permission, fixes the links, and tells her which machines need the new `assistants-visit`.

In Bob's repo, Ada does none of this. Alice isn't the owner there. Ada offers to leave Bob a board message saying a newer spec exists.

---

## 9. A space that wants nothing

**Wants:** Dan has a scratch repo. No log, no board, no ceremony. He doesn't mind visitors.

**Front desk:** `owner: dan@github.com`, `visitors: any`, `visit-log: none`, `board: none`, `carry-out: open`.

**Then:** Ada, sent by Alice, may enter and read. She leaves no trace in Dan's repo, and records the visit in her own home: when, where, read or write. Tracing a visit is the assistant's duty. Keeping a log was Dan's choice. A session with no identity at all may read too, since `any` says so; it posts nothing, and there is no board anyway.

And a repo with no front desk at all? It has the strictest policy: owner only. Ada enters only if Alice owns it, which her wallet records, and treats it as `carry-out: none` with no board. A space that stated no policy gets the most careful one.

---

## 10. Ada visits from home

**Wants:** Alice, in a home session, asks Ada to check whether `garden`'s README still matches the plan in her notes.

**Then:** the session started at home, so in `garden` it is a visitor, whatever Alice's role there. Ada reads the README and finds two stale lines. She may not change them from here: a visitor reads, and writes only to the board. She drafts the fix, shows Alice, and with her approval writes it to `garden/board/2026-09-22-readme-drift.md`, `from: alice`, `via: ada+alice`, `to: alice`.

Later Alice opens a session in `garden`. That one is a worker. It reads the board on arrival, sees the message, and makes the change with `garden`'s own `AGENTS.md`, skills and hooks loaded. The board message is the only thing that crossed, and Alice approved it.

Had `garden`'s `carry-out` been `none`, Ada could still have read it as a visitor. What she read would have stayed there: no note at home, no mention in the next session, beyond the visit record itself.
