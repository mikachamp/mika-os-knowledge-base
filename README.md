# Mika OS Knowledge Base

**An Obsidian vault that is both my second brain and the control plane for my AI agents.**

Most "agent frameworks" hide state in a database or a chat log. I put it in markdown files that I can open, read, search, edit and diff. Agents and I use the exact same files. If an agent claims a task, I see a lock file appear. If I want to change an agent's behaviour, I edit a note.

This repo is the **reusable template** (empty, with one example task and one example project) plus the write-up of how it works. It is the knowledge-base half of [mika-os](https://github.com/mikachamp/mika-os).

## What's in the template

| Path | Job |
|---|---|
| `AGENT_OPERATING_RULES.md` | The rules every agent follows: queue, claims, human gates |
| `Tasks/Queue/` | One markdown file per task, with YAML frontmatter (one example included) |
| `Tasks/Board.base` | An Obsidian Bases view of the queue: by status, by project, "my tasks", "agent queue (ready)" |
| `Tasks/locks/` | One lock file per claimed task; finished ones move to `locks/released/` |
| `Tasks/Results/` | `T-xxxx.md`: what was done, files, how it was tested, what is left |
| `Tasks/Reviews/` | Independent review notes with PASS/FAIL per done-test |
| `Tasks/Dispatch.md` | The lead's launch list plus a copy-paste sub-agent prompt |
| `Tasks/PAUSE.md.example` | Rename it to `PAUSE.md` and agents finish their step and stop |
| `Projects/` | One hub note per project (status, code folder, repo, owner) |
| `Agents/Sub-Agent Roles.md` | The role table, human version |
| `MOCs/` | Maps of content: one index note per category |
| `Notes/` | Playbooks and decision records (empty here) |
| `Resources/Design-Library/` | Structure for sites, tokens, patterns and screenshots, with two example patterns |

My own vault also holds project hubs, playbooks and logs. Those are private and are not in this repo.

## A task is just a file

```yaml
---
id: T-0012
title: Polish the landing page hero
project: "[[Some Project]]"
role: web-builder             # which sub-agent kind picks it up
assignee: builder             # or "mika"
status: queued                # queued | working | blocked | review | done
priority: 2
depends_on: [T-0010]
touches: [<project-folder>/Some-Project/]   # the only places it may change
done_test: "Opens on a 390 px phone with no sideways scroll"
claimed_by:
claimed_at:
lease_until:
result:
---
What to do, in plain steps. Links to source files.
```

Three fields do most of the work:
- **`touches`** is a fence. Overlapping tasks run in sequence, never together.
- **`done_test`** is the exit door. A task is not done until its test is met and I say so.
- **`depends_on`** keeps a builder from starting something whose prerequisite is open.

## One source of truth for humans and agents

- **Claiming** is atomic: an agent creates `Tasks/locks/T-xxxx.lock` with exclusive create. If the file exists, the task is taken.
- **Leases and heartbeats** mean a crashed agent does not hold a task forever. A lock whose lease expired more than 10 minutes ago can be taken over by the orchestrator.
- **Finishing** means writing `Tasks/Results/T-xxxx.md` and setting `status: review`. Only a human sets `done`.
- **Reviews** are separate notes written by a different agent, and a failed review goes back to the builder.
- **Human gates:** publishing, pushing, sending, deleting and spending are never an agent's call.

## The Design Library

A folder structure for design work, with a note template per kind:
- **Sites:** a 12-section note per studied site (structure, sizes, timing, treatment), with the source URL and the date it was checked.
- **Tokens:** colours, type scale, spacing, radii, easing and durations, with estimates marked `(est.)`.
- **Patterns:** one note per reusable idea (effort S/M/L, behaviour, when to use it). Two examples are included.
- **Screenshots:** the studied original vs my own version at 1440 px and 390 px.
- **Rule:** studies are for learning and are never presented as my own work. My own patterns are kept separate and labelled.

### The study loop
I rebuild layouts I admire, such as the four study builds in [web-lab](https://github.com/mikachamp/web-lab) (`design-studies/`, from a tutorial; the designs are not mine). My AI agents then break each study into patterns and tokens, and those notes grow the library. Then I apply the patterns in my own work, like [concept-sites](https://github.com/mikachamp/concept-sites). Study, document, apply.

My Three.js playbook (techniques and gotchas for 3D pages) lives in my private vault. The lessons from it are in [web-lab](https://github.com/mikachamp/web-lab).

## Why this works (and where it does not)

- **Good:** human-readable, diffable, no schema migrations, tool-agnostic. Obsidian shows it as a board; scripts see plain files.
- **Good:** the same note can be a task, a spec and an audit trail.
- **Limit:** lock files are a convention. This is a single-machine system and not a distributed one.
- **Limit:** it only works if agents are told to read the rules first. The rules file is the real product.

## Use the template

1. Copy this repo and open the folder in Obsidian (File, Open folder as vault). Enable the core Bases plugin for `Tasks/Board.base`.
2. Edit `AGENT_OPERATING_RULES.md` (your limits, your human gates).
3. Create a project hub in `Projects/`, then a task file in `Tasks/Queue/` (copy the example).
4. Give your agent the task file, the hub, and the rules file. Tell it to stop at `review`.
5. Read the result note, run the `done_test` yourself, and only then set `done`.

For the agent side (orchestrator, claim/heartbeat code, role registry), see [mika-os](https://github.com/mikachamp/mika-os).

Build story: [docs/BUILD-STORY.md](docs/BUILD-STORY.md).
