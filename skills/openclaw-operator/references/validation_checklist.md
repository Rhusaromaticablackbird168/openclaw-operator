# Validation Checklist

Use this checklist before declaring an OpenClaw-related task complete.

The goal is not to run every command blindly. The goal is to run the **smallest set of checks that proves the change works**.

## How to use this file

1. Identify the kind of change that was made.
2. Run the core validation checks that apply.
3. Run the task-specific checks that apply.
4. Record exactly what was validated and what remains unverified.

---

## 1) Pre-validation sanity check

Before validating, confirm:

- you know which files changed
- you know which subsystem was touched
- you know what “working” should mean for this task
- you are validating the actual environment that was modified

Useful quick checks:

```bash
pwd
git status --short 2>/dev/null || true
command -v openclaw || true
```

If relevant, also capture:

```bash
ls -la
find . -maxdepth 3 \( -name 'openclaw.json' -o -name 'SKILL.md' -o -name 'AGENTS.md' -o -name 'CLAUDE.md' \) 2>/dev/null
```

---

## 2) Core validation checks

Run the smallest applicable subset of these first.

### Base health

```bash
openclaw status
openclaw gateway status
openclaw doctor
```

### Skills

```bash
openclaw skills list
openclaw skills list --eligible
openclaw skills check
```

### Channels / integrations

```bash
openclaw channels status --probe
```

### Docs / command discovery

```bash
openclaw docs <topic>
```

### Logs when something is unclear

```bash
openclaw logs --follow
```

---

## 3) Task-specific validation

### A. Skill creation or skill update

Use when:

- a new skill was created
- an existing skill was edited
- skill discovery or eligibility was part of the task

Checks:

```bash
openclaw skills list
openclaw skills list --eligible
openclaw skills info <skill-name>
openclaw skills check
```

Confirm:

- the skill is discoverable
- the skill name is correct
- the description is clear enough for triggering
- supporting files resolve correctly
- no obviously broken markdown/frontmatter exists

If CLI validation is unavailable:

- confirm the folder structure is correct
- confirm `SKILL.md` exists at the expected path
- confirm YAML frontmatter is present and valid
- confirm referenced files actually exist

---

### B. Config or provider changes

Use when:

- model/provider config was changed
- runtime config was edited
- environment behavior depends on config correctness

Checks:

```bash
openclaw status
openclaw doctor
openclaw gateway status
```

Confirm:

- the config parses cleanly
- the provider/model identifiers match what the environment expects
- no secret values were accidentally written to tracked files
- the service reports healthy state after the change

If relevant, also inspect the exact changed config file.

---

### C. Channel or integration changes

Use when:

- auth settings changed
- a channel was connected or repaired
- an external integration was modified

Checks:

```bash
openclaw channels status --probe
openclaw gateway status
openclaw doctor
```

Confirm:

- the specific integration appears healthy or at least improved
- credentials were not leaked into files or logs
- the change affected only the intended integration
- there is a clear next step if manual auth is still required

---

### D. Cron or scheduled automation changes

Use when:

- a cron job was added or modified
- a schedule definition changed
- automation wiring was introduced

Confirm:

- the schedule exists in the right place
- the referenced skill/script/file actually exists
- the schedule is readable and unambiguous
- naming makes the job easy to identify later
- failure behavior is understandable
- time zone assumptions are explicit when relevant

If the environment supports it, also check that the automation is discoverable through the appropriate OpenClaw command or config inspection.

---

### E. Repo/file-structure changes

Use when:

- files were added, moved, renamed, or removed
- new support directories such as `assets/` or `references/` were introduced

Checks:

```bash
find . -maxdepth 4 -type f | sort
```

Confirm:

- all referenced files exist
- relative paths inside markdown/docs are correct
- no empty directories are relied on without a placeholder file
- the layout is understandable to a future coding agent

---

### F. Troubleshooting / repair work

Use when:

- the goal was diagnosing or repairing a broken installation
- no major new feature was added
- success means “health restored” rather than “new asset created”

Suggested order:

```bash
openclaw status
openclaw status --all
openclaw gateway probe
openclaw gateway status
openclaw doctor
openclaw channels status --probe
```

Confirm:

- the original failure symptom changed in the expected direction
- the probable root cause is identified or narrowed down
- the fix is smaller than a reinstall unless reinstall was justified
- unresolved risks are called out explicitly

---

## 4) Security and hygiene checks

Always check these before finishing:

- no secrets were pasted into markdown, JSON, scripts, or committed config
- no remote one-liner was introduced without explanation
- no unrelated files were changed accidentally
- no large rewrite happened where a narrow edit would have worked
- any copied snippet was reviewed before use

Useful quick check:

```bash
git diff --stat 2>/dev/null || true
git diff 2>/dev/null || true
```

---

## 5) What to report back

Every completion message should clearly state:

1. **What I found**
   - what environment or repo structure was discovered
   - what existing files/configs/skills were relevant

2. **What I changed**
   - exact files added or edited
   - exact behavior intended by the change

3. **How I validated it**
   - exact commands run
   - exact file/path checks performed
   - what passed versus what could not be verified

4. **What remains uncertain**
   - manual auth steps still needed
   - commands unavailable in the environment
   - runtime validation not yet possible
   - risks worth checking next

---

## 6) Minimal validation examples

### New skill added

```bash
openclaw skills list
openclaw skills info openclaw_operator
openclaw skills check
```

### Provider config updated

```bash
openclaw status
openclaw gateway status
openclaw doctor
```

### Channel repair

```bash
openclaw channels status --probe
openclaw gateway status
```

### New support folders/files added

```bash
find skills/openclaw-operator -maxdepth 3 -type f | sort
```

---

## 7) Completion standard

A task is complete when:

- the requested change was actually made
- the most relevant validations were run
- the result was summarized clearly
- remaining uncertainty was stated plainly
