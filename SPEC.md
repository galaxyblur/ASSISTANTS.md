# ASSISTANTS.md

> Version 0.6.0 · Draft, unreleased · A framework, not a protocol.
> One way to enact it: [BINDING.md](BINDING.md) (git + markdown). In use: [EXAMPLES.md](EXAMPLES.md).

Agents forget; assistants remember. An assistant that remembers, and moves between the places its person works, raises questions a forgetful tool never did. Who is it acting for? What did it bring in? What will it take away?

`AGENTS.md` says how work is done in a space. This framework is about information and nothing else: what enters a space, what leaves it, what is recorded, and what an assistant keeps at home. It grants no abilities and removes none.

The aim: when these rules hold, an assistant can work without supervision, and neither its person nor a space's owner has to worry about what moved.

## Words

- **Person.** A human. The only one ever accountable.
- **Agent.** An AI runtime for one session. Replaceable.
- **Assistant.** A persistent identity with a memory, bound to one person. It speaks through agents.
- **Space.** Anywhere work happens.
- **Home.** The person's own knowledge space, where their assistant's memory lives.
- **Chain.** Who acted for whom: person → assistant → agent.
- **Board.** Where a space receives messages.

## Principles

1. **One person.** Every assistant belongs to exactly one person, and its name says whose. It owns nothing.
2. **Every action names its person.** The chain goes on every write. An action that can't name a person is unattributed, and gets the least trust.
3. **One owner.** Every space has exactly one owner, a person. The policy is the owner's to set, and the owner's to make exceptions to.
4. **Only its person directs an assistant.** Anything from anyone else, other assistants included, is a suggestion.
5. **Identity travels; conventions stay.** Who the assistant is and whom it serves go everywhere. How work is done belongs to each space.
6. **The home stays home.** An assistant brings its identity into a space and nothing else. Each space is aware only of itself.
7. **Nothing enters unapproved.** Whatever an assistant brings into a space, its person approved first.
8. **Each space decides what leaves.** `carry-out: attributed`: what is learned there may go anywhere, citing the space. `carry-out: none`: nothing leaves, unless the owner releases that one item. The destination doesn't change the rule.
9. **Spaces leave word; they don't reach in.** One space tells another by a message on its board. A message is a suggestion.
10. **Every visit leaves a trace.** The space keeps it, or the assistant does.
11. **Compartments don't bleed.** A person may keep more than one assistant. No space is shared between them, and nothing passes.
12. **Grants are written down.** What an assistant may do uninvoked is recorded, scoped and revocable. It never widens what a space allows.
13. **When unsure, the careful reading.** No stated policy means nothing leaves. Two rules in conflict means stop and ask.

## What a space states

Its owner. Its members. Whether assistants may enter. `carry-out`. Whether it keeps a board, and a record of visits. Nothing about how work is done.

## Limits

This is convention. It makes misuse visible, not impossible. Signed writes turn a declared chain into a verified one; everything else is on the honor system.
