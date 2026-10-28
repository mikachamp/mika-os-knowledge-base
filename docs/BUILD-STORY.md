# How the knowledge base became a control plane

## Where it started: a second brain

The vault began as a place for notes: projects, coding references, learning logs, a design library. Each category got a map of content (MOC) so I could find things, and a home page linking them. The agents wrote their own logs (briefings, research, conversations) into functional subfolders, and the MOCs link into those instead of moving them.

## The turn: the vault became the queue

On **2026-10-23** I decided the vault should be the one place where work is tracked, for me and for every agent. That decision shaped everything after it:

1. **One folder per concern.** `Tasks/Queue`, `Tasks/Results`, `Tasks/locks`, `Graphs`.
2. **The old backlog became one file per task.** A single long list is where parallel agents collide, so a task became a file with frontmatter: owner, status, dependencies, files it may touch, and a test for "done".
3. **A hub note per project**, with fields for status, code folder, repo, graph report, portfolio page and owner. The hub is the first thing an agent reads after the rules.
4. **`Board.base`**, an Obsidian Bases table over `Tasks/Queue` with four views (by status, by project, my tasks, the agent queue that is ready to go). I open it and see everything in one screen.
5. **Graph reports as single notes.** I had once exported a note per code dependency into my vault. It drowned the graph view, so now it is one report per repo.

## Making agents safe to share it with

A shared vault with several writers needs rules. These came in this order:

- **Lock files** with exclusive create, so two agents cannot claim the same task.
- **Leases and heartbeats**, so a dead agent does not hold a task forever.
- **`touches` fences** and "one writer per file".
- **Working copies** in a `_worktrees/` folder for edits to existing files, because most of my web files are not in git and a worktree would not contain them.
- **A role registry** so a task's `role:` decides which kind of sub-agent runs it, with its own reading list and test standard.
- **A review note per batch**, written by an agent that did not do the work.

## The Design Library, and why it has a hard rule

I study good sites by measuring them: structure, sizes, timing, tokens. Then I build my own version. The notes are the product. The studies are not. Hence the rule at the top of the library: **studies are for learning, they are not my work.** Each site note holds its source URL and check date, and anything I estimated rather than measured is tagged `(est.)`.

From studies I extract **patterns** worth reusing, and I keep my *own* patterns in a separate table so the line between "studied" and "mine" stays visible.

### The study loop

The four luxury layouts in [web-lab](https://github.com/mikachamp/web-lab) (`design-studies/`) are study builds from a tutorial, and the designs are not mine. Their value is the loop: I rebuild a layout, my AI agents break it into patterns and tokens, and those notes grow the design library. Then the patterns get applied in my own work, such as [concept-sites](https://github.com/mikachamp/concept-sites). The library gets better each time a study goes through it.

## What went wrong

- **A note per dependency.** Exporting a graph node per file flooded my vault. Fix: one report per repo.
- **Backup files left lying in live folders** after parallel edits. A review caught them and I now check for them before anything is shared.
- **Two timestamp formats.** One lead wrote local-offset times, another wrote UTC. A lease check could misread one. The helper code now parses both and writes UTC.
- **Stale locks.** Locks stay while a task waits at `review`, which makes the board look busy. It is by design (released on approval), but I now treat it as something to note.

## What I would tell someone copying it

Start with just three things: a `Tasks/Queue` folder, one task template, and a rules file that says "stop at `review`". Add locks when you have two agents, not before. Add roles when you notice yourself repeating the same instructions.
