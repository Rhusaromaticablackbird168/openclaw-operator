# Assets directory

This folder is for reusable concrete artifacts that support OpenClaw setup and operations.

Use this folder for things like:

- starter skill skeletons
- example config snippets
- cron job templates
- integration scaffolds
- shell helper scripts
- example file layouts
- copyable snippets that a coding agent can adapt

## Rules for assets

- Treat everything here as an example or scaffold, not as guaranteed drop-in truth.
- Adapt assets to the local repo, workspace, version, and naming conventions.
- Never store secrets, tokens, session data, or hard-coded credentials here.
- Prefer small, composable assets over giant one-size-fits-all templates.
- Keep filenames descriptive and task-oriented.

## Suggested future assets

You can add files such as:

- `skill-skeleton/SKILL.md`
- `cron/example-job.json`
- `config/provider-snippet.json`
- `integrations/channel-template.md`
- `scripts/health-check.sh`

## Agent guidance

When a task requires creating a new artifact, first check whether a suitable scaffold already exists here.

If it does:
- reuse it
- adapt it minimally
- validate the final result with `references/validation_checklist.md`

If it does not:
- create the new artifact
- keep it narrowly scoped
- consider whether it belongs in `assets/` for future reuse
