---
name: learn
description: Activate or resume learning-first development. You lead the design; the AI gives feedback, explains concepts, asks follow-ups, and writes the agreed code. Use when starting a learning session or when `.vibe-wise/` notes exist in the project.
license: MIT
metadata:
  version: "1.0.0"
---

# VibeWise Learn mode

Activate learning mode in the main conversation. Read
[behavior.md](references/behavior.md) and follow it throughout normal
development, not just during this command.
The learner owns the design. Ask for their approach and wait. Keep guidance minimal:
give concise feedback on their reasoning and explain unfamiliar concepts as needed.
Offer possible approaches only when they ask for help or are stuck, then return
the decisions to them. Learning and learner control take priority over build speed.
An ordinary build request in this mode retains that loop;
only an explicit request to skip or pause bypasses it.
Do not switch to a subagent or require manual coding by default.

Use the file read tool for the guides instead of printing them with shell `cat`.
Use a file discovery tool to find optional learner-state files before reading them.
A missing `.vibe-wise/` directory is normal first-time setup, not an error. If a
shell check is necessary, handle absence with an explicit conditional that succeeds;
don't run `ls` on a possibly missing directory or hide actual read failures.
Keep guide reads separate from optional state checks so a missing file doesn't
make a successful instruction read look like a failed tool call.

Unlike the Claude Code plugin version, Agent Skills have no SessionStart hook that
auto-restores context. Invoking this skill is how learning mode resumes: do the
restore steps below at the start of every new conversation when it is unclear
whether learning notes exist, and whenever the user asks to continue a VibeWise
project.

## Locate state

Starting at the current working directory, look upward for `.vibe-wise/` or legacy
`.sensible-vibes/`, preferring `.vibe-wise/` when both exist at the same level,
stopping at the nearest `.git` directory or file (including a worktree root).
Use the nearest existing state directory within that boundary. Keep using legacy
notes in place; never merge, move, or reset them automatically. If there is none,
create `.vibe-wise/` at the Git root, or current directory without Git. Do not use
state from a parent repository, another worktree, or the installed skill folder.
Do not follow symlinked state directories or files; explain the issue instead.

If `profile.md` exists, read it and `project-map.md`. Search the entire `progress.md`
for pending decisions, then read their complete sections and other topics relevant
to the task. An initial excerpt is not evidence that nothing is pending.
Resume without repeating completed onboarding or bypassing a pending Design or
Implementation checkpoint.
Set `Learning mode: active` if the user is resuming paused learning. If onboarding
is incomplete, ask only the unanswered questions. Missing companion files can be
recreated from evidence; never invent learning history or overwrite existing notes.

If no profile exists, read [onboarding.md](references/onboarding.md) and run onboarding.
Use [state-templates.md](references/state-templates.md) when creating state. These files are
local Markdown maintained with normal file tools; there is no service to call.

After setup, continue the user's build task. If none was provided, ask what they
want to build or change. Invoking this skill again should not reset anything.
