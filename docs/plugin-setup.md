# Finsider for Claude, Codex and Cursor

The Finsider plugin lets your agent work Finsider Actions the way an engineer
works tickets: it picks up an Action (a step of a diligence engagement), works
it with Finsider's verified tools, and its session shows live on the Finsider
Actions board (Working, Needs input, Finished) under that task. Your team
reviews the work and moves it to Done.

Your Finsider account decides which companies the agent can see. Each agent
signs in to Finsider in your browser the first time (OAuth); no CLI, key or
password is stored in the plugin.

## One click, from Finsider

Open **Actions** in Finsider and choose **Connect agents**:

- **Add to Claude** opens Claude's Add connector dialog with Finsider filled in
  (Claude on the web, desktop and mobile). Confirm and sign in.
- **Add to Cursor** opens Cursor with Finsider ready to install. Confirm and sign in.
- **Install in Codex** installs the plugin once Codex knows this marketplace
  (see below).

Then open any task and choose **Send to Claude**, **Codex** or **Cursor**: the
agent opens with the work already written as its prompt.

## Claude Code

```bash
claude plugin install finsider --marketplace finsider-ai/releases
```

(Older Claude Code: `claude plugin marketplace add finsider-ai/releases`, then
`claude plugin install finsider@finsider --scope user`.) Start a new session and
run `/mcp` to sign in to Finsider. `/finsider:work Harbor Dental` starts work.

## Codex

```bash
codex plugin marketplace add finsider-ai/releases
codex plugin add finsider@finsider
```

Codex asks you to sign in to Finsider when the plugin installs. After the
marketplace is added once, **Install in Codex** in Finsider installs updates in
one click.

## Cursor

Use **Add to Cursor** in Finsider, or add this marketplace in Cursor's plugin
settings. Cursor signs in to Finsider in the browser.

## What the agent can and cannot do

The agent reads Actions, puts its own session on the board and reports
progress. Every Finsider change it prepares (an upload, a sync, a verification
run, a report) shows you its exact preview and waits for your approval.
Finishing a session never ticks a task: your team moves it to Done.

## Without remote sign-in

Hosts that cannot sign in remotely can use the Finsider CLI bridge: install the
CLI from [home.finsider.ai/agents](https://home.finsider.ai/agents), run
`finsider auth login`, then `finsider mcp config codex` or
`finsider mcp config claude` and run the printed command.
