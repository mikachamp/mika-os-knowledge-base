---
type: dispatch
updated: YYYY-MM-DD
---
# Dispatch: which sub-agent tasks to run, and how
> The lead agent's launch list. Keep it in sync with the task files; the task file always wins.

## Run mode
- Which tools run the sub-agents (for example a cloud coding assistant), and anything that must stay switched off.
- Maximum tasks in `working` at once across all leads: **3** to start with.

## Lead checklist (every launch)
1. Check `Tasks/PAUSE.md` does not exist.
2. Pick a task from "Ready now": `status: queued`, every `depends_on` is `done`, `touches` does not overlap a task in `working`, assignee is not the human.
3. Claim it:
   - create `Tasks/locks/T-xxxx.lock` (exclusive create, so two agents cannot both win);
   - write your name and the time inside;
   - in the task file set `status: working`, `claimed_by`, `claimed_at` and `lease_until` (now + 30 min).
4. Make a working copy of anything that will change (`_worktrees/T-xxxx/`).
5. Launch a sub-agent of the task's `role` with the prompt template below.
6. When it reports, review it: scope, screenshots, facts checked against sources. Fixes go back to the same sub-agent.
7. Report to the human in one message: done, blocked, needs OK.
8. After the human approves: copy the working copy back, set `status: done`, move the lock to `Tasks/locks/released/`.

## Sub-agent prompt template
```text
You are a sub-agent doing exactly ONE task: <T-xxxx title>. Work autonomously, then stop.
Read first:
1. AGENT_OPERATING_RULES.md (rules; they override everything)
2. Your task file in Tasks/Queue/ (already claimed for you; status: working)
3. The project hub in Projects/
Limits:
- Change ONLY the files listed in the task's `touches`.
- Never publish, push, commit, send, delete or contact anyone.
- Facts only from the notes and files; never invent numbers, names or dates.
Finish: write Tasks/Results/T-xxxx.md (what changed, files, how tested, what is left),
set status: review, then stop and report in 5-10 lines.
```

## Ready now
| Task | Role | Notes |
|---|---|---|

## Waiting for the human
| Task | Blocked by | Notes |
|---|---|---|

## Done
| Task | Result |
|---|---|
