# ThinkCNAP Security Skills

> Run an AWS security assessment from your terminal, in minutes — and ship the results straight to [thinkcnap.org](https://thinkcnap.org).

A Claude Code slash command (`/assess-aws`) that walks your AWS account through the [think-cnap](https://thinkcnap.org) security framework, scores each control with read-only AWS CLI checks, and submits the maturity scores to your thinkcnap.org dashboard.

## Why use it

- **No agents to install.** Read-only AWS CLI calls, scoped by the AWS managed `SecurityAudit` policy.
- **Any domain, any control.** Drill down to the exact measures you care about — or assess everything.
- **Real evidence, not guesses.** Every score has a CLI command or a direct user answer behind it; ambiguous evidence is skipped, not invented.
- **Credentials stay in your environment.** Tokens and keys are read from env vars only — never prompted, never written to disk.
- **Context persists between runs.** Tooling answers and business context are cached locally (and git-ignored) so you don't repeat yourself.

## Quick start

```bash
export THINKCNAP_API_TOKEN="..."
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_DEFAULT_REGION="eu-west-1"
```

Then in Claude Code:

```
/assess-aws
```

Full usage, the 8-step flow, and the project rules live in [`CLAUDE.md`](./CLAUDE.md).

## What's in the repo

| Path | Purpose |
|---|---|
| `.claude/commands/assess-aws.md` | The `/assess-aws` slash command |
| `memory/maturity-scale.md` | 0–3 scoring rubric and field rules |
| `memory/thinkcnap-api.md` | thinkcnap.org API contract (source of truth) |
| `memory/user-profile.md` | Cached tooling/context/findings (git-ignored) |

## License

MIT — do what you like, no warranty.
