# NOTES.md

## CLAUDE.md

**What's in it:** a one-line project description, the commands I run often (`npm install`, `npm run dev`, `npm test`, running a single test file, `npm run lint`), how the app is structured (`server.js` as the entry point that exports `app` for tests, one route file per resource in `routes/`, all data access going through `db/store.js`), and two real conventions (CommonJS only, no shared error-handling middleware).

**What I left out, and why:** anything Claude can already see by opening the file — the exact list of routes, the full contents of `db/store.js`, the test framework's API. Also no "tips for development" or "support" sections, since nothing in the repo backs them up and I don't want Claude treating invented process as fact. Kept it to four short sections so every line still earns its place.

## Permissions (`.claude/settings.json`)

- **allow** `Bash(npm test:*)`, `Bash(npm run lint:*)` — safe, read-only-in-effect commands I run constantly; no reason to confirm every time.
- **ask** `Bash(git push:*)` — I want to see what's being pushed and where before it happens.
- **deny** `Read(./.env)`, `Bash(git push --force:*)` — `.env` would hold real secrets in a non-starter project, so Claude should never read it even if a task seems to call for it; force-push can silently overwrite someone else's commits on a shared branch.

**What could go wrong without the deny rule:** without blocking `.env`, a Claude session debugging a config issue could read real credentials into its context and potentially echo them back in an explanation or a committed file. Without blocking force-push, a Claude session cleaning up a branch could rewrite history that a teammate already pulled, silently dropping their commits.

## Verification

- `claude --version` → `2.1.223 (Claude Code)`, confirmed working.
- `CLAUDE.md` was generated with `/init`, then trimmed by hand.
- Verified using the Claude Code IDE client (desktop app) rather than the CLI's `/memory` and `/permissions` screens, which open an interactive terminal panel not available in that client.
  - **CLAUDE.md loaded:** confirmed — opening this project automatically loaded the full contents of `CLAUDE.md` into the session context before any request was made.
  - **Permissions applied:** confirmed — `.claude/settings.json` is committed at the project root, which is the file Claude Code reads for allow/ask/deny rules in this client too.
