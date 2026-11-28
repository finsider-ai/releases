# Finsider

Financial diligence worked the way software is built: every engagement is a
board of Actions, and Claude, Codex or Cursor pick them up like tickets. Your
agent reads the Action, works it with Finsider's verified tools, and its session
shows live on the Actions board (Working, Needs input, Finished) under that
task. The team reviews the work and moves it to Done.

This package carries manifests for Claude Code (`.claude-plugin`), Codex
(`.codex-plugin`), Cursor (`.cursor-plugin`) and the portable Agent Plugins
format (`plugin.json`), the hosted gateway as a remote MCP server
(`https://agents.finsider.ai/mcp`, OAuth with your Finsider account, no CLI or
key needed), two skills (`actions` for the ticket workflow, `diligence` for the
evidence work) and the `/finsider:work` command. It contains no credentials.

## Install

- **Claude (web, Desktop, mobile):** in Finsider open Actions, Connect agents,
  Add to Claude, then confirm and sign in.
- **Claude Code:** `claude plugin install finsider --marketplace finsider-ai/releases`
- **Codex:** `codex plugin marketplace add finsider-ai/releases`, then
  Install in Codex from Finsider (or `codex plugin add finsider@finsider`).
- **Cursor:** Add to Cursor from Finsider's Connect agents.

Each agent signs in to Finsider in the browser the first time and acts as you:
it sees the companies you can see, never ticks a task, and every Finsider change
it prepares waits for your approval.

## Use

Open a task in Finsider and choose Send to Claude, Codex or Cursor, or ask:
"Work the next open Finsider Action on Harbor Dental." In Claude Code and
Cursor, `/finsider:work Harbor Dental` does the same.

The `finsider mcp serve` CLI bridge remains available for hosts that cannot use
remote OAuth: install the CLI, run `finsider auth login` and point the host at
`finsider mcp serve`.
