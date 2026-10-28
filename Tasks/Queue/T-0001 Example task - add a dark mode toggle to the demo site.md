---
id: T-0001
title: "Example task: add a dark mode toggle to the demo site"
project: "[[Example Project]]"
assignee: builder            # an agent name, or "mika" (the human owner) for decisions and approvals
role: web-builder            # which sub-agent role runs it, see Agents/Sub-Agent Roles
status: queued               # queued | claimed | working | blocked | review | done
priority: 2                  # 1 high, 2 normal, 3 low
depends_on: []               # task ids that must be done first
touches: ["example-site/"]   # the only files or folders this task may change
done_test: "Toggle works on a 390 px phone and at 1440 px; no console errors; no sideways scroll"
claimed_by:
claimed_at:
lease_until:
result:
created: 2026-01-01
source: "Fictional example: where the request came from"
---
Why: a short reason in plain English (this file is a template, the content is made up).

Steps:
1. Copy the page into the working folder named in `touches`.
2. Add a toggle button in the header; store the choice in `localStorage`.
3. Test at 1440 px and 390 px, then write `Tasks/Results/T-0001.md` and set `status: review`.

Only the human owner sets `status: done`.
