# The canonical install block

One install story, one wording. `README.md` and every page under `docs/` must use the commands below. Change them here first, then propagate.

## Editable skills (skills.sh)

Use skills.sh for Claude Code, Codex, Copilot, and other compatible agents. The installer copies editable skill files. Select the skills you want, including `setup-matt-pocock-skills` for the engineering workflow. Updates are manual.

<canonical-block name="other-agents">

```bash
npx skills@latest add mattpocock/skills -a <agent>  # omit -a to choose your agent
```

When the installer asks which skills to take, include `setup-matt-pocock-skills`. To update, run `npx skills@latest update`, and re-run `add` to pick up new skills.

</canonical-block>

## Gemini CLI

The `--path` option takes one bucket, so install each promoted bucket separately.

<canonical-block name="gemini">

```bash
gemini skills install https://github.com/mattpocock/skills.git --path skills/engineering
gemini skills install https://github.com/mattpocock/skills.git --path skills/productivity
```

Re-run both commands to update.

</canonical-block>

## One skill

Use the single-skill form wherever one skill is named on its own. `docs/` pages do not use this block, because ai-hero renders the install widget above the page body. See [writing-docs.md](./writing-docs.md).

<canonical-block name="skills-sh-one-skill">

```bash
npx skills@latest add mattpocock/skills --skill=<name>
```

```bash
npx skills@latest update <name>
```

</canonical-block>

`skills@latest` is the pinned spelling everywhere.
