# security-skills

Claude Code slash command for running AWS security assessments against the [think-cnap](https://thinkcnap.org) framework.

## What's here

```
.claude/commands/assess-aws.md   # The /assess-aws slash command
memory/maturity-scale.md         # 0–3 scoring rubric, field rules
memory/thinkcnap-api.md          # thinkcnap.org API contract (source of truth)
memory/user-profile.md           # tooling answers, business context, prior findings
                                 # (git-ignored; created/updated by the skill, never holds credentials)
```

## Using `/assess-aws`

### 1. Prerequisites

- A thinkcnap.org account with an API token (thinkcnap.org → Integrations → API Token)
- An AWS IAM user or role with the AWS managed policy `arn:aws:iam::aws:policy/SecurityAudit` attached
- The AWS region you want to assess

### 2. Export credentials into the environment

The skill reads credentials only from environment variables. It does not prompt for them and will not read or write them to any file.

```bash
export THINKCNAP_API_TOKEN="..."
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_DEFAULT_REGION="eu-west-1"
```

In the Claude Code prompt you can also run `! export THINKCNAP_API_TOKEN=… AWS_ACCESS_KEY_ID=… AWS_SECRET_ACCESS_KEY=… AWS_DEFAULT_REGION=…` to set them for the current session.

### 3. Invoke the skill

Type `/assess-aws` in Claude Code. The skill will:

1. Verify all four env vars are set (stops with instructions if any are missing).
2. Fetch the available domains/controls/measures from thinkcnap.org.
3. Verify AWS access with `aws sts get-caller-identity`.
4. Ask which domain(s), control(s), and measure(s) to assess (drill-down).
5. For each measure, run read-only AWS CLI checks or ask a targeted tooling question, score against `memory/maturity-scale.md`, and append findings to `memory/user-profile.md`.
6. Ask for business context once per month (industry, team size, tooling, timeframe) to set `desired_maturity`.
7. Show a summary table and let you edit values before submitting.
8. POST results to thinkcnap.org via the contract in `memory/thinkcnap-api.md`.

### 4. After the run

- Review the full assessment at [thinkcnap.org](https://thinkcnap.org).
- Findings and context persist in `memory/user-profile.md` for the next run — the skill re-asks tooling/context if they're older than one month.

## Rules

- **Credentials live in env vars only.** Never commit them, never write them into any file under `memory/`, never echo them — mask as `***` if they must be referenced.
- **`memory/thinkcnap-api.md` is the source of truth for the thinkcnap.org API.** The skill reads from it; don't duplicate endpoints or schemas inline.
- **Scoring uses `memory/maturity-scale.md`** for all measures.

## Installing the skill in another project

These are local Claude Code slash commands — no deployment needed. Copy or symlink `.claude/commands/assess-aws.md` (and the `memory/` files it depends on) into any project where you want `/assess-aws` available.
