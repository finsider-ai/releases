# Finsider for Claude Code and Codex

The Finsider plugin adds financial diligence tools and the bundled diligence workflow to your local AI client. Your Finsider account determines which companies and workspaces you can access.

## Sign in first

Install Finsider CLI 1.1.0 or newer using the [CLI setup instructions](https://home.finsider.ai/agents). Then run:

```bash
finsider --version
finsider auth login
```

Complete sign-in in your browser. The plugin starts `finsider mcp serve` automatically using that signed-in account; you do not need to run the bridge yourself.

## Claude Code on macOS or Linux

Run these commands in your terminal:

```bash
claude plugin marketplace add finsider-ai/releases
claude plugin install finsider@finsider --scope user
```

Start a new Claude Code session. Use `/mcp` to check the Finsider connection.

## Codex on macOS or Linux

Run these commands in your terminal:

```bash
codex plugin marketplace add finsider-ai/releases
codex plugin add finsider@finsider
```

Start a new Codex session and confirm Finsider appears in the available tools.

These commands use the public Finsider marketplace in this repository. Access to a private Finsider source repository is unnecessary. See the official [Claude Code plugin guide](https://code.claude.com/docs/en/discover-plugins) and [Codex plugin documentation](https://developers.openai.com/plugins/build/plugins) for host settings and workspace policies.

## Windows PowerShell

Use the CLI-generated connection command for your AI client. Automatic marketplace MCP startup on Windows is still being verified.

```powershell
finsider.cmd auth login
# Choose the client you use:
finsider.cmd mcp config codex
finsider.cmd mcp config claude
```

Copy and run the setup command printed for your chosen client once in PowerShell, then start a new client session. The generated command uses the correct executable paths for your Windows installation. Keep this standalone connection separate from the marketplace plugin so a second Finsider MCP server is not enabled. The [diligence workflow](../plugins/finsider/skills/diligence/SKILL.md) is available as a reference alongside the connection.

## Use Finsider

Ask: “Use Finsider to assess diligence readiness for my selected company and period.” Finsider gathers evidence before proposing changes. Review each proposed mutation and its exact preview before approving it.

If sign-in expires, run `finsider auth login` again. Keep the CLI installed on the same computer as your AI client. If you already configured a standalone Finsider MCP connection, use one connection method to avoid duplicate servers.

## ChatGPT and Claude web

Remote web connection availability is pending. Those clients require additional Finsider-provided OAuth setup and compatibility checks; installing this local plugin does not enable a remote web connector.

The hosted MCP endpoint is `https://agents.finsider.ai/mcp`. See [custom connection setup](https://home.finsider.ai/agents) for current availability.
