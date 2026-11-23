---
title: What is MAESTRO?
description: MAESTRO is a desktop agent builder — describe a task, and MAESTRO builds, checks, runs, repairs and evolves an agent (a chain). The TUI and the CLI remain as secondary interfaces.
sidebar:
  order: 1
---

**MAESTRO** turns a task description into a chain of model calls, tools and other steps. You can test the chain on examples, save it and run it on new inputs. The [CARL format](/carl/getting-started/overview/) describes its steps and dependencies.

Start by installing the skill for your agent. It helps you set up the CLI and work with chains from your chat. [Install the MAESTRO skill](/care/skill/install/).

## Interfaces

| Interface | Command | Status |
| --- | --- | --- |
| Agent skill | — | Drive MAESTRO from another agent through the CLI. See [Agent skill](/care/skill/overview/). |
| **MAESTRO desktop app** | `maestro` (or `maestro app`) | A native window with a chat agent, My agents, the IdeaL panel, Activity and Settings. |
| Textual TUI | `maestro tui` | Supported, secondary. Terminal chat with Interactive and Production modes. |
| Headless CLI | `maestro <subcommand>` | Supported, secondary. Scripting, CI, Memory administration. |

The Python package is `maestro-care`; its import name and older command name is
`care`. `care` still works as an alias of `maestro`.

## What the desktop app does

The app is built around the **MAESTRO harness**: a resident chat agent with
sessions, tools, approvals and background jobs. Chain work is done by typed
MAESTRO tools that the agent calls on your behalf.

- **Build** an agent from a description. By default the chat agent writes the
  chain itself, checks real API responses and test-runs it; the MAGE
  generator is the alternative. Before building it asks for what is missing,
  or decides and states its assumptions.
- **Run** it on your input or attached files (PDF, Word, Excel, PowerPoint,
  text); pause, resume or stop it between steps.
- **Repair** a failed run: MAESTRO proposes a corrected chain; running it is a
  separate step.
- **Save** agents to memory and find them later in
  [My agents](/care/app/library/).
- **Prepare evaluation data**: import a CSV/XLSX file or generate a checked
  dataset in the [dataset studio](/care/autobench/overview/).
- **Evolve** an agent on that dataset with the evolution platform, with exact
  metrics, an LLM judge or several objectives at once (a Pareto front).
- **Work with ideas** in [IdeaL](/ideal/getting-started/overview/), shown
  inside the app: seed ideas, see which ones help evolution, push an idea
  into a running evolution.
- **Create tools and skills** in the workshop, with checks before saving.

By default all model calls use the **chat model you selected in the
session**; Settings can switch chain work to a separate model. Paid work asks
for one confirmation in an app dialog. See [Confirmations](/care/app/confirmations/).

Many of these features are in builds from the current development line and
not yet in a tagged MAESTRO release; the [app pages](/care/app/overview/)
mark them.

## The stack

| Part | Role |
| --- | --- |
| **MAESTRO** (`maestro-care`) | The app, the harness plugins, the CLI and the TUI. |
| **MAGE** (`mmar-mage`) | Generates a CARL chain from a task description (the alternative to the chat agent writing it). |
| **CARL** (`mmar-carl`) | Executes chains. See [CARL](/carl/getting-started/overview/). |
| **GigaEvo Memory** with **IdeaL** | Stores chains, memory cards, tools and skills; IdeaL stores ideas and their effect on evolution. |
| **GigaEvo Platform** | Evolves a chain against a dataset. |

Details: [Architecture](/care/concepts/architecture/).

## Where to go next

- [Quick start](/care/getting-started/quick-start/): install, open the app, set the model, build the first agent.
- [The desktop app](/care/app/overview/): sessions, chat, My agents, IdeaL, Activity, Settings.
- [Release 0.5.0](/care/reference/release-0-5-0/): what this release contains and its known limits.
- [CLI reference](/care/cli/overview/) and [TUI](/care/tui/overview/) for the secondary interfaces.
