---
name: agent-plugin-setup
description: Creates and validates a plugin project that has agent skills, agent files, skill evals, and plugin evals for Claude Code and GitHub Copilot. Use when the user asks to set up, scaffold, check, or release an agent plugin or an agent skill project.
compatibility: Requires Node.js 22.12 or later. Uses the claude CLI (version 2.1.289 or later) and skills-ref (a Python package) for validation. The plugin evals also need Git 2.31 or later.
---

# Agent plugin setup

Use this skill to build a plugin project with one or more skills and agent files for Claude Code and GitHub Copilot. The examples in the reference files use placeholders such as `<plugin-name>` and `<skill-name>`. Replace each placeholder with the name that the user gives.

## Workflow

1. Ask the user for the plugin name, the skill names, and the purpose of each skill. Ask which skills need an agent file.
2. Read `references/rules.md`. Check the plugin name against the rules. Do not use a reserved name.
3. Read `references/structure.md` to see the target folder tree.
4. Do the procedure in order. Read each file only when you reach its Part:
   - Parts A and B, folders and manifests: `references/procedure-scaffold.md`
   - Part C, skills: `references/procedure-skills.md`
   - Part D, agent files: `references/procedure-agents.md`
   - Parts E and F, skill evals and plugin evals: `references/procedure-evals.md`
   - Parts G and H, validation, release, and the final checklist: `references/procedure-release.md`
5. After each Part, check the result before you start the next Part.
6. Write all new text in the style of ASD-STE100 Simplified Technical English. Use short sentences, the imperative form, and the active voice.
7. Write scripts in Node.js. Use the `.mjs` extension. Do not write Python. Shell scripts are allowed only where `references/rules.md` says so.

## Validation

Run these commands from the project root:

```bash
skills-ref validate skills/<skill-name>
claude plugin validate . --strict
```

Run `claude plugin eval . --scaffold --allow-tools Bash Write Edit` only when the user agrees. This command makes model calls and costs money.

## Gotchas

- Use the same `name` in `plugin.json`, `.claude-plugin/plugin.json`, and the marketplace entry. An install by name fails if the names differ.
- Keep the same `version` in both manifests. Do not set `version` in the marketplace entry.
- Claude Code reads `agents/`. Copilot reads `com.github.copilot/agents/`. Each client ignores the other folder.
- Claude Code runs commands in the working directory of the session, not in the skill folder. Write the path to a bundled script with `${CLAUDE_SKILL_DIR}`. Use `${CLAUDE_PLUGIN_ROOT}` only for files that several skills share. Copilot defines no variable for skill text.
- Grant `Bash` to the plugin evals only with `--allow-tools`. Do not list it in `prompt.md`.
- Add no packages. The plugin must have no `package.json`.

## Sources

Read `references/sources.md` to find the official documents that this skill follows.
