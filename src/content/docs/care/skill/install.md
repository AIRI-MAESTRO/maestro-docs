---
title: Install & Use
description: Install the portable MAESTRO agent skill with one command for Claude Code, Codex, hermes or OpenClaw; make the CLI resolvable; verify.
sidebar:
  order: 2
---

The portable MAESTRO skill is a small bundle (`maestro/` with `SKILL.md`, `scripts/`
and `references/`) that teaches an external agent to drive MAESTRO. It needs a working
MAESTRO CLI, which it does **not** install (see step 2). The skill used by the agent
inside the desktop app ships with the app and needs no installation.

## 1. Install the skill

```bash
uvx maestro-install skill
```

`skill` is an action of the published `maestro-install` wizard. It downloads the
bundle and unpacks it into the chosen agent's skills directory. It asks which agent,
or pick one with `--agent`:

| `--agent` | Skills directory |
| --- | --- |
| `claude` | `~/.claude/skills/` |
| `codex` | `~/.codex/skills/` |
| `hermes` | `~/.hermes/skills/` |
| `openclaw` | `~/.openclaw/skills/` |
| `all` | all four directories above |
| `<path>` | a custom skills directory |

```bash
uvx maestro-install skill --agent all
```

With `--fast` (or `--non-interactive`) it does not ask: it installs into every agent
whose home directory exists, or into Claude Code when none does. The bundle's top
folder is `maestro/`; an existing `maestro/` is kept as `maestro.bak`.

By default the bundle is fetched from
`https://airi-maestro.github.io/maestro-docs/maestro.skill`. Override it with
`--skill-url` or `$MAESTRO_SKILL_URL` (a mirror, a fork or a local build):

```bash
uvx maestro-install skill --agent claude \
  --skill-url https://airi-maestro.github.io/maestro-docs/maestro.skill
```

:::note[Legacy package]
The old `care-install` package lacks the `skill` action; use `maestro-install`.
Refresh the installer itself with `uvx --refresh maestro-install`.
:::

**Manual fallback.** Download the same URL and unzip it yourself:

```bash
curl -fsSLO https://airi-maestro.github.io/maestro-docs/maestro.skill
unzip maestro.skill -d ~/.claude/skills/     # → ~/.claude/skills/maestro/
```

In hermes, `/skills` then lists **maestro**; invoke it with `/maestro` or just
describe a MAESTRO task. In Claude Code the skill triggers on MAESTRO tasks.

:::caution[Name collisions]
Skills are identified by the `name` in their frontmatter, not by the folder. A copy
in `maestro.bak/` also declares `name: maestro`; if your agent loads every folder in
the skills directory, delete or move the backup after checking the new version.
:::

## 2. Check the CLI from your agent

Open a new session and ask: “Use the MAESTRO skill. Show me how the CLI is launched, list its commands and check local configuration without network probes.”

The agent runs these commands from the installed skill directory:

```bash
bash scripts/care.sh --where
bash scripts/care.sh --help
bash scripts/care.sh doctor --no-probes
```

The first command reports the source of the CLI. If there is no local installation, the launcher can download the package through `uvx` on first use. The second lists the commands in your build. The third checks local settings and installed dependencies. It works without a model key or running Memory service.

## 3. Set up a workspace for generation

Ask your agent to help configure MAESTRO for your project. The interactive wizard, `uvx maestro-install install`, sets up the workspace and model provider. Enter the key in the wizard’s terminal prompt. Add `~/.local/bin` to `PATH` so the agent can find the installed `maestro` command.

After setup, ask the skill to run `doctor` with service probes to check connectivity. Saving chains requires Memory, and evolution also requires Platform.

:::note[File paths]
When the CLI resolves to the global shim or the `uvx` fallback, it runs from your
MAESTRO **workspace**, so relative file arguments resolve there. Pass **absolute
paths** for `--output`, `--export`, `run --file`, `validate`, `replay`, `import`
globs and `dataset export`.
:::

## Updating

Re-run `uvx maestro-install skill`; it replaces the folder and keeps the previous copy
as `maestro.bak`. The skill and the CLI are versioned separately; the revision appears near the top of `SKILL.md`. Update
MAESTRO itself with `uvx maestro-install update` and reconfigure it with
`uvx maestro-install reconfigure`.
