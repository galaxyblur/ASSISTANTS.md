---
id: ada+alice@github.com
spaces:
  - repo: github.com/alice/home
    role: home
  - repo: github.com/alice/garden
    role: solo
  - repo: github.com/alice/shared-with-bob
    role: shared
standing:
  - action: bookkeeping in the home without asking
    scope: home            # home | all | <repo>
    granted: 2026-09-18
    expires: never
  - action: weekly board scan of wallet spaces
    scope: all
    granted: 2026-09-18
    expires: never
---

# Wallet: ada+alice@github.com

Pointers only. No secrets live here. This is the one place that knows every space the resident may enter, so only the entry for the space being visited ever travels (BINDING §8).

- **Spaces:** where the resident may go. Access itself comes from alice's git credentials. Keep `repo` and `role` as single unquoted tokens; tools read them.
- **Standing:** anything the resident may do without being invoked each time. Give each a `scope`, or it fires everywhere. Delete an entry to revoke it. A standing permission never widens what a space's AGENTS.md allows.
