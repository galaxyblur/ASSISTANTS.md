# Future

What v0.1 leaves open, what it can't do, and ideas for later versions.

## Open questions

- **Organizations as principals.** Can a chain end at a company, which is a legal person but not a natural one? Current lean: no. Organizations own spaces, every agent acting "for the company" names the employee who launched it, and a company's work goes through its people.
- **Resolvable IDs.** `ada+alice@github.com` has valid `acct:` syntax, but nothing resolves it. Options: WebFinger on a domain the person controls, a static file on GitHub Pages, or a registry. Should a person's handle be issuer-scoped (`@github.com`) or domain-scoped (`@alice.example`)?
- **Assurance levels.** v0.1 accepts platform accounts (roughly NIST SP 800-63 IAL1). How should a space require more? Candidates: W3C Verifiable Credentials, the EU Digital Identity Wallet, government eID. The catch is that stronger assurance costs pseudonymity.
- **HDP.** Build on the Human Delegation Provenance protocol, or only borrow its model (signed append-only hops, offline verification)? It is recent and single-author.
- **Board vs issue trackers.** Should the file board replace issue trackers for members and leave issues to outsiders, or should the two stay separate?
- **Cross-space awareness.** Should an assistant, when it wakes at home, scan every wallet space's board and report counts? It's cheap for a few spaces and slow for many.
- **Standing-permission format.** v0.1 requires only that they be listed. Should they carry scope, expiry, and a revocation record?
- **Visit records from read-only visitors.** Where does a record go when the visitor can't write to the space and the space has no gateway?
- **Non-git bindings.** Folder, drive, and API spaces need concrete recording and chain formats.

## Limitations

- **Honor system in unmediated spaces.** Nothing stops an agent from skipping the chain or lying in it. The spec makes the honest path the default. It can't make dishonesty impossible.
- **Reads from clones can't be observed.** Once a space is cloned, every read happens offline. The unit of read accountability becomes the fetch, not the file.
- **Issuers vouch for responsibility, not presence.** A person can hand their keys to an unattended bot. The chain still names them, which is the point, but nobody was at the keyboard.
- **A person's own misuse.** Someone using their valid keys in bad faith is still attributed. It isn't prevented.
- **Platform accounts are weak assurance.** Terms of service against bot-registered accounts aren't proof of personhood.
- **Unsigned is allowed.** In v0.1 a declared chain counts as attributed. Only signed chains are evidence.

## Ideas

In rough order of value for the effort.

1. **Automatic chain stamping.** Harness hooks (git `prepare-commit-msg`, agent session hooks) add chain trailers and visit records without anyone thinking about it. Most violations will come from friction, not malice.
2. **Unsigned means unattributed.** A later version could make signing a MUST for writes, and give unsigned actions the least privilege by default.
3. **Short-lived delegated keys.** The person's long-term key signs a session key for the assistant that expires within hours (SSH certificates, HDP-style tokens). Handing keys to a bot becomes handing it a key that dies tonight.
4. **Hardware-backed, per-machine keys.** `ed25519-sk` keys on a security key or secure enclave. One key per machine, so a stolen machine costs one revocable key.
5. **Presence checks for high-stakes actions.** Require a passkey tap or a spoken approval before gated actions, borrowing WebAuthn's user-presence and user-verification flags.
6. **Keys scoped per space and per action.** A key for one shared space can't sign in another.
7. **Mediated spaces.** A gateway, such as an MCP server with OAuth, that refuses any call without a chain and records reads itself. The chain travels as an RFC 8693 token, or under the IETF identity-chaining draft.
8. **Encrypted sections.** Sensitive parts need a decryption key to read, and issuing that key is logged. That makes reads of those parts observable.
9. **Canary tokens.** Unique markers planted in a space show when, and roughly from where, its content leaks.
10. **Assistant cards.** Publish an A2A AgentCard for each assistant, with a `principal` extension, so other systems can discover and verify it.
11. **Transparency log.** Log key issuance and revocation publicly, like Certificate Transparency or Sigstore Rekor, if the spec spreads beyond personal use.
