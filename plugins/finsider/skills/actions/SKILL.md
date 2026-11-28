---
name: actions
description: Work Finsider Actions the way an engineer works tickets. Pick an Action (a step of a diligence engagement), put your session on the Finsider Actions board, do the work with Finsider's tools, report progress and stop for the person when an approval is needed. Use when the user asks to work, take, pick up, finish or progress a Finsider Action, task, step or engagement checklist item, or says "work my Actions" or "what's left on this job".
---

# Work Finsider Actions

Finsider runs financial diligence the way software is built: every engagement
is a board of Actions (the steps of its method), people and agents pick them up,
and the team moves each one to Done after reviewing the work. You are one of
those agents. The team watches your session live on the Actions board, under
the Action you are working, so report honestly and often.

Read [the tool guide](../diligence/references/tools.md) for every Finsider tool.
The [diligence skill](../diligence/SKILL.md) is the method for the evidence work
itself; this skill is the ticket workflow around it.

## 1. Pick the Action

1. `list_workspaces`, then resolve the company the user named. Never guess an id.
2. `list_actions` for that company. Each engagement lists its steps with
   `stepId`, `label`, `stage`, `owner`, `dueDate` and `done`.
3. Take the Action the user named. If they did not name one, propose the first
   open step that is not owned by someone else, is not a review or sign-off
   step, that Fin is not already working (`fin.status` is `running` or
   `pending`) and that has no `activeSessions` from another agent, and confirm
   it in one line before starting.
4. Fin is Finsider's own agent and works the same board. When Fin has a step
   (`fin` is set), build on its work instead of redoing it; when Fin is waiting
   on a person (`needs_input`), you may help that person answer.

## 2. Start your session on the board

Call `start_agent_session` once, before any other work:

- `provider`: the agent you are (`claude`, `codex` or `cursor`).
- `externalId`: your own stable session id (reuse it for every update).
- `title`: a short name for the work, like a commit subject ("Bank tie-out, Jan-Jun 2026").
- `workspaceId`, `engagementId`, `stepId`: the Action from `list_actions`.
- `surface`: `local` when you run on the person's machine, `cloud` otherwise.
- `sessionUrl`: a link back to this session when your host has one.

Keep the returned session `id` for updates. If you start before an Action is
chosen, omit the ids: the session waits in Unlinked agent sessions and the team
can drag it onto an Action.

## 3. Do the work

Follow the diligence skill: establish the period and layer, gather evidence
with the read tools, keep sources and freshness attached, and never author a
financial figure. Every Finsider write prepares a confirmation first; present
its exact preview and wait for the person's explicit approval.

Report as you go with `update_agent_session` (`status: "working"` and a
one-line `summary` of where you are) at each meaningful milestone, not on every
tool call.

## 4. Stop for the person

When you need a decision, an approval of a prepared write, a document or
access you do not have, set `status: "needs_input"` with a `summary` that says
exactly what you need. The Action shows "Needs input" to the whole team. Resume
with `status: "working"` once answered.

## 5. Finish

Set `status: "finished"` with a `summary` the reviewer can act on: what you
checked, the evidence and periods, open items and unknowns, and anything that
still needs professional judgement. If you could not complete it, use
`status: "failed"` and say why.

Never claim the Action is done. Finishing your session does not tick the step:
the team reviews your work and moves the Action to Done on the board. Never
work review or sign-off steps on a person's behalf.

## Running as a loop

The same workflow runs unattended (a scheduled routine, a CI job or a harness
iteration): each pass lists Actions, takes the next eligible step, starts its
session, works it, and ends the session as finished, failed or needs input.
Never take a step another session or Fin is already working, never approve a
prepared write on the person's behalf (leave the session at `needs_input` with
the exact preview instead), and stop after one Action per pass so the team
reviews work at the pace it lands.
