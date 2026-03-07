# OpenClaw Operator Reference Playbooks

Use this file only when the main `SKILL.md` is not enough.

This file contains practical task playbooks for common OpenClaw setup, configuration, and troubleshooting work. It is meant to complement the main skill, not replace it.

## How to use this file

1. Start with the main `SKILL.md`.
2. Identify the task category you are working on.
3. Use the matching playbook below.
4. Apply the smallest safe change.
5. Validate with `references/validation_checklist.md` before declaring success.

---

## 1) First-pass environment inspection

Use this at the start of almost any OpenClaw-related task.

### Goals

- determine whether you are in a source repo, an installed workspace, or both
- locate the active config and relevant files
- confirm whether the `openclaw` CLI is available
- understand the current state before making edits

### Suggested commands

```bash
pwd
ls -la
find . -maxdepth 3 \( -name 'openclaw.json' -o -name 'SKILL.md' -o -name 'AGENTS.md' -o -name 'CLAUDE.md' \) 2>/dev/null
command -v openclaw || true
```

If the CLI exists:

```bash
openclaw status
openclaw gateway status
openclaw skills list --eligible
```

### What to capture

- current working directory
- repo root if applicable
- workspace path if applicable
- relevant config files
- relevant skill directories
- whether runtime commands succeed

### Common mistakes to avoid

- editing before locating the real active config
- assuming the current directory is the live workspace
- creating duplicate skills instead of updating the existing one
- skipping status checks and guessing at runtime state

---

## 2) Create a new custom skill

Use this when the user wants a new OpenClaw skill for a recurring task or domain.

### Goals

- create a discoverable skill with a clear trigger description
- keep the skill focused and easy for agents to use
- separate core instructions from supporting material

### Recommended structure

```text
<skill-folder>/
├── SKILL.md
├── references/
│   ├── validation_checklist.md
│   └── task-playbooks.md
└── assets/
    └── README.md
```

### Steps

1. Check whether a skill already exists for the same purpose.
2. Create a narrow skill folder with a descriptive name.
3. Add `SKILL.md` with:
   - YAML frontmatter
   - a concise description of when to use the skill
   - a default operating loop
   - safety and validation expectations
4. Put long examples or optional guidance in `references/`.
5. Put reusable scaffolds in `assets/`.
6. Validate skill discovery.

### Frontmatter guidance

The frontmatter should clearly answer:

- what is this skill for?
- when should the agent use it?
- what kinds of tasks are in scope?

### Validation

```bash
openclaw skills list
openclaw skills list --eligible
openclaw skills info <skill-name>
openclaw skills check
```

### Common mistakes to avoid

- vague descriptions that never trigger the skill
- putting all detail in `AGENTS.md` or `CLAUDE.md` instead of the skill
- using a broad scope like “handles all OpenClaw tasks”
- forgetting to validate that the skill is actually discoverable

---

## 3) Update an existing skill

Use this when a skill already exists and only needs refinement.

### Goals

- preserve the existing working structure
- improve clarity without bloating the main file
- avoid breaking references or trigger descriptions

### Steps

1. Read the full existing `SKILL.md`.
2. Identify whether the change belongs in:
   - the description/frontmatter
   - the main operating loop
   - supporting references
   - reusable assets
3. Prefer narrow edits over full rewrites.
4. Keep descriptions concise and trigger-oriented.
5. Re-check all referenced files and paths.

### Validation

```bash
openclaw skills list
openclaw skills info <skill-name>
openclaw skills check
```

### Common mistakes to avoid

- replacing a good structure with a much larger but less usable one
- breaking relative links to `references/` or `assets/`
- adding too much session-specific detail to the skill

---

## 4) Add or modify provider/model configuration

Use this when the task involves model providers, model aliases, or runtime model settings.

### Goals

- understand the existing provider configuration before changing it
- make a minimal config change
- confirm the runtime still reports healthy state

### Steps

1. Locate the active provider/model config.
2. Inspect the existing naming and structure.
3. Confirm whether the change is:
   - adding a provider
   - switching a model
   - updating parameters
   - fixing a broken config
4. Make the smallest possible edit.
5. Check for accidental secret exposure in tracked files.
6. Validate health.

### Validation

```bash
openclaw status
openclaw gateway status
openclaw doctor
```

### Common mistakes to avoid

- changing provider names inconsistently
- editing the wrong config file
- storing secrets in committed config instead of the appropriate secret mechanism
- changing multiple providers at once without need

---

## 5) Add or repair a channel/integration

Use this when connecting or debugging an OpenClaw channel or external integration.

### Goals

- determine whether the issue is config, auth, reachability, or runtime state
- repair the specific integration without disturbing others
- avoid leaking credentials while debugging

### Steps

1. Inspect current status first.
2. Identify the specific integration being changed.
3. Review relevant config and any existing docs in the repo/workspace.
4. Make only the necessary auth or config fix.
5. Re-check the exact integration.
6. Note any manual user step still required.

### Validation

```bash
openclaw channels status --probe
openclaw gateway status
openclaw doctor
```

### Common mistakes to avoid

- reconnecting everything when only one channel is broken
- printing credentials into shell history or markdown files
- assuming auth is valid without probing status afterward

---

## 6) Add or modify a cron/scheduled job

Use this when the task involves repeated or time-based OpenClaw work.

### Goals

- decide whether the schedule belongs inside OpenClaw or an external scheduler
- create a readable and auditable schedule
- ensure the referenced target exists and is valid

### Steps

1. Determine the scheduling mechanism already used in the environment.
2. Confirm whether the job belongs in OpenClaw-native scheduling.
3. Give the job a descriptive name.
4. Keep the schedule explicit and readable.
5. Ensure all referenced files, skills, or scripts exist.
6. Document time zone assumptions if relevant.
7. Validate discoverability and file references.

### What to verify

- schedule path/location is correct
- target skill/script exists
- names are understandable later
- failure behavior is known
- schedule timing is not ambiguous

### Common mistakes to avoid

- mixing external cron and OpenClaw-native scheduling with no clear ownership
- using cryptic names
- creating a schedule that depends on a missing skill or script
- forgetting time zone assumptions

---

## 7) Troubleshoot a broken installation

Use this when the system is not working and the goal is diagnosis or repair.

### Goals

- isolate the failing layer
- gather the minimum evidence needed
- prefer the smallest viable repair

### Suggested diagnostic order

```bash
openclaw status
openclaw status --all
openclaw gateway probe
openclaw gateway status
openclaw doctor
openclaw channels status --probe
openclaw logs --follow
```

### Diagnostic approach

1. Confirm the reported symptom.
2. Determine whether failure is at the level of:
   - config
   - gateway
   - channel/integration
   - skill discovery
   - provider/model
   - OS/service/runtime
3. Identify the probable root cause.
4. Apply the smallest fix first.
5. Re-run the relevant health checks.

### Common mistakes to avoid

- reinstalling before checking health/status/doctor output
- changing several layers at once
- treating logs as the starting point instead of a targeted follow-up
- declaring success before the original symptom is re-tested

---

## 8) Add new support files or folders to a skill

Use this when expanding a skill with `references/`, `assets/`, scripts, or templates.

### Goals

- keep the main skill small and high-signal
- move bulky or reusable material into the correct support area
- preserve a clean and understandable folder structure

### Rules of thumb

- `SKILL.md` = main instructions and operating loop
- `references/` = optional deeper guidance, playbooks, validation, long examples
- `assets/` = reusable scaffolds, snippets, starter files, helper artifacts

### Steps

1. Decide whether the new content is instructional or reusable.
2. Put instructional content in `references/`.
3. Put reusable artifacts in `assets/`.
4. Update `SKILL.md` so the agent knows the file exists and when to use it.
5. Check all relative paths.

### Validation

```bash
find . -maxdepth 4 -type f | sort
```

### Common mistakes to avoid

- putting templates into `references/` when they belong in `assets/`
- creating support files but never mentioning them in `SKILL.md`
- relying on empty directories with no placeholder file

---

## 9) Create coding-agent wrapper files

Use this when wiring the OpenClaw operator skill into Codex or Claude Code.

### Goals

- keep always-loaded root guidance concise
- route the coding agent toward the main skill
- avoid duplicating the entire skill in multiple files

### Strategy

- put the full operating guidance in `skills/openclaw-operator/SKILL.md`
- keep `AGENTS.md` short and directive
- keep `CLAUDE.md` short and directive
- tell the coding agent when it should consult the skill

### Common mistakes to avoid

- copying the whole skill into `AGENTS.md`
- copying the whole skill into `CLAUDE.md`
- letting the wrapper files drift out of sync with the actual skill

---

## 10) Good completion summary template

Use this format after finishing work:

### What I found

- environment type: source repo, workspace, install, or combination
- relevant files, configs, and skills discovered
- current health signals or failure symptoms

### What I changed

- exact files added or edited
- exact intended effect of each change

### How I validated it

- exact commands run
- exact file/path checks performed
- what passed
- what could not yet be verified

### Remaining risks or next steps

- any manual auth still needed
- any runtime limitation in the current environment
- any follow-up validation worth doing later

---

## 11) Minimal quick-start checks by task type

### New skill

```bash
openclaw skills list
openclaw skills info <skill-name>
openclaw skills check
```

### Config or provider update

```bash
openclaw status
openclaw gateway status
openclaw doctor
```

### Integration/channel change

```bash
openclaw channels status --probe
openclaw gateway status
```

### File/folder structure update

```bash
find . -maxdepth 4 -type f | sort
```

### Troubleshooting

```bash
openclaw status
openclaw gateway status
openclaw doctor
```

---

## 12) Final reminder

This file is a supporting reference.

The default behavior should still be:

1. inspect before editing
2. make the smallest safe change
3. validate the result
4. report clearly what was found, changed, and verified
