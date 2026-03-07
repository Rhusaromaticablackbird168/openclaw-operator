# OpenClaw Operator

Give Codex and Claude Code the context to set up, validate, and troubleshoot your OpenClaw installation.

<p align="left">
  <img alt="OpenClaw" src="examples/screenshots/hero.png" width="100%" />
</p>

> Bring your coding agent to your local OpenClaw install, describe what you want, and let it do the heavy lifting:
>
> - create or update skills
> - wire providers and integrations
> - add cron/scheduled automation
> - inspect config safely
> - validate changes
> - troubleshoot broken installs

## Why this exists

OpenClaw is powerful, but setup can still be intimidating.

A lot of the friction is not “writing code” — it’s knowing:
- where the right files live
- which commands to run first
- how to validate changes safely
- how to structure skills and supporting files
- how to troubleshoot without random trial-and-error

**OpenClaw Operator** gives coding agents like **Codex** and **Claude Code** the missing operational context.

Instead of manually piecing together docs, configs, skills, cron jobs, and validation steps, you can point your coding agent at your OpenClaw setup and say what you want.

## What it does

This repo provides:

- a reusable `SKILL.md` for OpenClaw operations
- a `validation_checklist.md` for safe completion and verification
- a `task-playbooks.md` reference for common workflows
- a lightweight `AGENTS.md` for Codex
- a lightweight `CLAUDE.md` for Claude Code
- a structure you can copy into your own repo or local OpenClaw install

## Who this is for

This is for you if you:

- run OpenClaw locally
- want to use Codex or Claude Code to configure it
- want a repeatable structure for OpenClaw setup work
- are tired of re-explaining OpenClaw conventions to your coding agent every session

## What this is **not**

This is **not** a replacement for OpenClaw itself.

It does not magically install or configure every environment without review.

It is an **operator pack**: a set of instructions, playbooks, and validation habits that help a coding agent work on an OpenClaw installation more effectively.

## Demo

<p align="left">
  <img alt="OpenClaw Operator demo" src="examples/screenshots/demo.gif" width="100%" />
</p>

### Example prompt

```text
Inspect this OpenClaw installation, identify why the provider config is failing, fix it with the smallest safe change, and validate the result before finishing.
```

### Example outcomes

With this pack installed, your coding agent should be better at tasks like:

- “Create a new OpenClaw skill for my local media workflow.”
- “Add a scheduled job and validate that it’s wired correctly.”
- “Inspect this install and tell me why the gateway/channel is failing.”
- “Set up provider config safely and confirm the health checks pass.”

## Repo structure

```text
openclaw-operator/
├── README.md
├── LICENSE
├── AGENTS.md
├── CLAUDE.md
├── docs/
│   ├── install.md
│   ├── demo-script.md
│   └── faq.md
├── examples/
│   ├── prompts/
│   │   ├── setup-provider.md
│   │   ├── create-skill.md
│   │   ├── add-cron-job.md
│   │   └── troubleshoot-install.md
│   └── screenshots/
│       ├── hero.png
│       ├── flow-diagram.png
│       └── demo.gif
└── skills/
    └── openclaw-operator/
        ├── SKILL.md
        ├── assets/
        │   └── README.md
        └── references/
            ├── validation_checklist.md
            └── task-playbooks.md
```

## Quick install

You can use this pack in three places:

1. **Codex**
2. **Claude Code**
3. **OpenClaw itself**

You can install one, two, or all three depending on your workflow.

---

## Install for Codex

Codex reads project guidance from `AGENTS.md` and scans skills from `.agents/skills/`.

From the root of your repo:

```bash
mkdir -p .agents/skills
cp AGENTS.md ./AGENTS.md
cp -R skills/openclaw-operator .agents/skills/openclaw-operator
```

Then open the repo in Codex and prompt it with an OpenClaw setup task.

### Example Codex prompt

```text
Read AGENTS.md and the openclaw-operator skill, inspect this OpenClaw setup, and help me add a new cron job with validation.
```

---

## Install for Claude Code

Claude Code can use `CLAUDE.md` for persistent repo guidance and `.claude/skills/` for skills.

From the root of your repo:

```bash
mkdir -p .claude/skills
cp CLAUDE.md ./.claude/CLAUDE.md
cp -R skills/openclaw-operator .claude/skills/openclaw-operator
```

### Example Claude Code prompt

```text
Use the OpenClaw Operator skill to inspect this installation, fix the provider configuration with the smallest safe change, and validate it before finishing.
```

---

## Install into OpenClaw

OpenClaw loads skills from either:

- `<workspace>/skills`
- `~/.openclaw/skills`

You can install this skill into either location.

### Option A: workspace-local

```bash
mkdir -p ./skills
cp -R skills/openclaw-operator ./skills/openclaw-operator
```

### Option B: user-level

```bash
mkdir -p ~/.openclaw/skills
cp -R skills/openclaw-operator ~/.openclaw/skills/openclaw-operator
```

## Recommended setup

The best setup for most people is:

- put `AGENTS.md` in the repo root
- put `CLAUDE.md` in `.claude/CLAUDE.md`
- install the skill into:
  - `.agents/skills/openclaw-operator`
  - `.claude/skills/openclaw-operator`
  - optionally `~/.openclaw/skills/openclaw-operator`

That way:
- Codex can use it
- Claude Code can use it
- OpenClaw-native agents can use it too

## How it works

The core idea is simple:

1. your coding agent sees an OpenClaw-related task
2. it consults the `openclaw-operator` skill
3. it inspects the current state before editing
4. it makes the smallest safe change
5. it validates the result before declaring success

This pack is opinionated in a few useful ways:

- inspect before editing
- prefer official CLI/docs over memory
- keep changes narrow and reversible
- preserve existing structure and naming
- validate before finishing
- never leak secrets in logs, diffs, or files

## What’s inside the skill

### `skills/openclaw-operator/SKILL.md`

The main operating playbook.

Use it for:
- when to activate the skill
- how to inspect an OpenClaw environment
- how to approach edits safely
- how to report back

### `skills/openclaw-operator/references/validation_checklist.md`

The file that helps a coding agent verify work before saying “done.”

Use it for:
- health checks
- skill validation
- config validation
- channel/integration checks
- security/hygiene checks
- completion standards

### `skills/openclaw-operator/references/task-playbooks.md`

The file for common OpenClaw workflows.

Use it for:
- creating skills
- setting up cron/scheduled automation
- wiring providers
- troubleshooting installs
- common first-pass inspection flows

### `skills/openclaw-operator/assets/`

A place for reusable examples, templates, snippets, and scaffolds.

This is intentionally lightweight in v1, but it is designed to grow over time.

## Example prompts

### Create a new skill

```text
Read the OpenClaw Operator skill and create a new custom OpenClaw skill for managing my home server backups. Keep the skill concise, add any supporting references if needed, and validate the result.
```

### Add a scheduled task

```text
Use the OpenClaw Operator skill to add a scheduled OpenClaw task that runs every morning, summarize what files you changed, and validate that the schedule is wired correctly.
```

### Troubleshoot a broken install

```text
Inspect this OpenClaw installation, determine why it is failing, identify the most likely root cause, fix it with the smallest safe change, and show exactly how you validated the fix.
```

### Provider setup

```text
Use the OpenClaw Operator skill to inspect the current provider configuration, fix any obvious issues, and validate gateway health without exposing secrets.
```

## Safety and limits

This repo is designed to help coding agents work more safely, not more recklessly.

Please review sensitive changes carefully, especially when tasks involve:

- API keys
- auth flows
- channels/integrations
- system services
- scheduled jobs
- workspace-wide configuration

A coding agent should still be treated like an operator that needs review, not blind trust.

## Suggested workflow

Here’s the workflow I recommend:

1. clone this repo or copy the pack into your own repo
2. install the skill for the coding agent you use
3. open your real OpenClaw install/workspace in Codex or Claude Code
4. describe what you want in plain English
5. let the agent inspect the current state first
6. review the proposed changes
7. make sure it validates before finishing

## Screenshots

### Flow

<p align="left">
  <img alt="OpenClaw Operator flow" src="examples/screenshots/flow-diagram.png" width="100%" />
</p>

## FAQ

### Do I need all of the files?

No.

Minimum useful setup:

```text
skills/openclaw-operator/SKILL.md
skills/openclaw-operator/references/validation_checklist.md
```

Recommended setup:

```text
AGENTS.md
CLAUDE.md
skills/openclaw-operator/SKILL.md
skills/openclaw-operator/references/validation_checklist.md
skills/openclaw-operator/references/task-playbooks.md
```

### Do I need to install it inside OpenClaw too?

Not necessarily.

If your goal is to use **Codex or Claude Code** to work on an OpenClaw install, the most important thing is to install the pack where **your coding agent** can see it.

Installing it inside OpenClaw is optional, but useful if you want OpenClaw-native agents to have the same skill available.

### Is this only for advanced users?

No.

It is especially helpful for people who know what they want to accomplish but do not want to memorize OpenClaw conventions, file locations, and validation steps.

### Is this an official OpenClaw project?

No.

This is a community-created operator pack built to help coding agents work more effectively with OpenClaw.

## Roadmap

Planned improvements:

- starter assets and scaffolds
- more example prompts
- better screenshots/GIFs
- a short demo video
- a ClawHub-friendly distribution path
- more playbooks for integrations and troubleshooting

## Contributing

PRs and issues are welcome.

Good contributions include:

- better task playbooks
- safer validation flows
- clearer install instructions
- more realistic demo prompts
- improvements to the README visuals
- tested examples for real OpenClaw workflows

## License

MIT

---

## Credits

Built for the OpenClaw community and anyone who wants to turn OpenClaw setup into a more self-serve workflow with coding agents.

If this repo helps you, star it, share it, and send improvements back upstream.
