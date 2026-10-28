---
name: Sub-Agent Roles
type: agent-registry
status: template
tags: [meta, agent, system, subagents]
machine_readable: agents/shared/subagent-roles.json
---
# Sub-agent roles

> One registry for every agent system that reads the vault. Every task file has a `role:` field naming one role below, so whoever picks the task up knows which sub-agent to launch and what its limits are. The machine-readable copy is `agents/shared/subagent-roles.json` in the `mika-os` repo.

## Roles
| Role | Does | Model hint |
|---|---|---|
| `web-builder` | Websites and pages: concept sites, content updates | Strong coding model |
| `motion-3d-builder` | Three.js scenes, hero animations, logo intros, scroll motion | Strong coding model |
| `app-builder` | Electron and React Native apps | Strong coding model |
| `engine-developer` | Backend and engine code with tests | Strong coding model |
| `case-study-writer` | Write-ups from real code; every claim cites a file | Any |
| `design-librarian` | Design Library notes: sites, tokens, patterns, screenshots | Any |
| `researcher` | Read-only research; every fact has a source and a date | Any |
| `reviewer` | Independent check of a finished result against its `done_test`; never edits | A different model from the builder |
| `github-organizer` | Audits and organises a GitHub account with the `gh` CLI; read-only, every change needs the owner's OK | Strong model |
| `explorer` | Read-only search; returns paths and a short summary | Small, fast model |
| `scribe` | Result notes, status to `review`, releases locks | Small, fast model |
| `privacy-guard` | Read-only check of anything others will see: personal data, client data, consent | Small, fast model |
| `git-keeper` | Backup planner: secret scan, `.gitignore` check, the exact commands for the owner to run; never commits or pushes | Small, fast model |
| `human` | Decisions, approvals, contacting people, deleting. Never launch a sub-agent for these | - |

## Common limits (no role can change these)
- Change only files listed in the task's `touches` (or its working copy).
- Never publish, push, commit, send, delete or contact anyone.
- Facts only from the owner's notes and files; never invent numbers, clients or dates.
- Finish by writing `Tasks/Results/T-xxxx.md`, setting `status: review`, and stopping.
- If `Tasks/PAUSE.md` exists, stop and report.

## Running several of the same role
When a request covers several sites or variants, split it into one task per target (own `touches` each), give each the same `role:`, and run them in parallel up to the cap of 3 `working`. Overlapping `touches` run in sequence via `depends_on`. Each task gets its own working copy and its own review.

## Adding a new role
Draft it with `status: proposed` (what it does, reading list, test standard). A proposed role may only do read-only or notes-only work until the owner approves it once. Then set `status: active`.
