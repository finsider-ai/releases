# Connect Finsider to your AI tools

The Finsider plugin adds financial diligence tools and the bundled diligence workflow to your local AI client. Your Finsider account determines which companies and workspaces you can access.

Pick the section for the tool you use.

- Claude Code, Codex and Claude Desktop - connect today through the Finsider CLI on your computer.
- Claude on the web, Claude Cowork, ChatGPT and Grok - use the hosted connector, which is not live yet. See [Hosted connector](#hosted-connector-claude-web-claude-cowork-chatgpt-and-grok).
- Any agent that can save files on your computer - add documents to a company's Data Room through [Finsider Drop](#finsider-drop).

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

## Claude Desktop

Claude Desktop starts the same `finsider mcp serve` bridge from its config file. It doesn't read your terminal's PATH, so the config needs the full path to the Finsider CLI.

1. Install the CLI and sign in as described in [Sign in first](#sign-in-first).
2. Find the two paths Claude Desktop needs, first the Node executable and then the Finsider CLI entry point. It needs both, because the `finsider` command starts Node through your PATH, which Claude Desktop doesn't have.

   On macOS or Linux, run these two lines in your terminal and copy each path it prints.

   ```bash
   command -v node
   node -p "require('fs').realpathSync(process.argv[1])" "$(command -v finsider)"
   ```

   On Windows, run `finsider.cmd mcp config claude` in PowerShell. After the `--` in the command it prints come the same two paths in single quotes. Copy both without the quotes.
3. Open Claude Desktop and click **Settings**, then **Developer**, then **Edit Config**. This opens `claude_desktop_config.json`, which lives at `~/Library/Application Support/Claude/claude_desktop_config.json` on macOS and `%APPDATA%\Claude\claude_desktop_config.json` on Windows.
4. Paste this into the file, replacing the Node path with the first path you copied and the entry point with the second.

   ```json
   {
     "mcpServers": {
       "finsider": {
         "command": "/path/to/node",
         "args": ["/path/to/finsider/dist/bin.js", "mcp", "serve"]
       }
     }
   }
   ```

   On Windows, double every backslash in both paths, because JSON treats a single backslash as an escape.

   If the file already has an `mcpServers` block, add the `finsider` entry inside it instead of pasting a second block.
5. Save the file, quit Claude Desktop completely and open it again.
6. Start a new chat and check that Finsider appears in the chat's tools. Ask "List my Finsider workspaces" to confirm it answers with your companies.
7. If Finsider is missing, open **Settings**, then **Developer**, and read the error next to `finsider`. A wrong path is the usual cause, so repeat step 2. If it says you aren't signed in, run `finsider auth login` again and restart Claude Desktop.

A coming CLI release adds `finsider mcp config claude-desktop`, which prints this snippet with your paths already filled in. Until that release ships, copy the paths by hand as above. Use one connection method, so don't also add Finsider to Claude Desktop another way.

## Hosted connector: Claude web, Claude Cowork, ChatGPT and Grok

The hosted connector at `https://agents.finsider.ai/mcp` isn't live for these clients yet. Each person signs in with their own Finsider account, and every AI client needs its own pre-registered sign-in client on Finsider's side, which Finsider is setting up now. Adding the URL today won't give you a working connection, so use Claude Code, Codex or Claude Desktop in the meantime. This page will say when each client goes live and will list the sign-in details it needs.

Installing the local plugin doesn't enable the hosted connector. See [custom connection setup](https://home.finsider.ai/agents) for current availability.

### Claude on the web and Claude Cowork

Claude Cowork uses the connectors on your Claude account, so one connector serves Claude on the web, in the desktop app and in Cowork. Once the Claude sign-in client is live, the steps will be these.

1. In Claude, open **Customize**, then **Connectors**, then click **+** and **Add custom connector**. On a Team or Enterprise plan, an owner first adds the connector under **Organization settings**, then **Connectors**, and each member then connects their own account.
2. Enter `Finsider` as the name and this URL.

   ```text
   https://agents.finsider.ai/mcp
   ```

3. Open **Advanced settings** and enter the OAuth client ID that Finsider will publish on this page. Click **Add**.
4. Click **Connect**, sign in with your Finsider account and approve access.
5. Start a new chat, turn Finsider on for the conversation and ask "List my Finsider workspaces" to confirm it works.
6. If sign-in fails, check that the client ID matches this page exactly, then contact Finsider support with the error Claude shows.

Remote connectors supply tools only. Upload the bundled [diligence skill](../plugins/finsider/skills/diligence/SKILL.md) where your plan allows skills, or paste its workflow into your request.

### ChatGPT

ChatGPT also needs access tokens tied to the Finsider connector, which Finsider is resolving with its sign-in provider before ChatGPT goes live. Once it is live, the steps will be these.

1. In ChatGPT, open **Settings**, then **Security and login**, and turn on **Developer mode**. Your workspace policy may not allow it, in which case ask your ChatGPT admin.
2. Open **Plugins** and click **+**.
3. Enter `Finsider` as the name, a short description and this MCP URL.

   ```text
   https://agents.finsider.ai/mcp
   ```

4. Choose OAuth and enter the client details Finsider will publish on this page.
5. Complete the Finsider sign-in and approve access, then review the tools ChatGPT discovered.
6. Add Finsider to a new chat and ask "List my Finsider workspaces" to confirm it works.
7. If sign-in fails, check the client details against this page, then contact Finsider support with the error ChatGPT shows.

### Grok

Finsider hasn't registered or tested a Grok sign-in client yet, so the exact Grok menu names aren't confirmed. Once it is live, the steps will follow the same pattern.

1. Open the place in Grok where you add a custom MCP connector.
2. Add a connector named `Finsider` with the URL `https://agents.finsider.ai/mcp` and the client details Finsider will publish on this page.
3. Sign in with your Finsider account and approve access.
4. Ask "List my Finsider workspaces" to confirm it works.

This section will get click-by-click steps once the Grok client is registered and tested.

## Finsider Drop

Finsider Drop lets any agent that can save files on your computer add documents to a company's Data Room, including Claude Code, Codex and Claude Cowork once you give it the folder. It only uploads, and nothing downloads from Finsider to your disk.

1. Install the Finsider desktop app from the [latest release](https://github.com/finsider-ai/releases/releases/latest), open it and sign in.
2. The app makes a `Finsider Drop` folder in your home folder with one folder per company you can add documents to, named after the company. Open it from the app menu with **Open Finsider Drop Folder**.
3. Save or copy a file into that company's folder.

   ```text
   ~/Finsider Drop/<company>
   ```

   On Windows this is `C:\Users\<you>\Finsider Drop\<company>`.
4. Check that the file moves into the `Uploaded` folder inside the company folder and shows up in that company's Data Room.
5. If it moves into `Not uploaded` instead, the upload failed or the file is over 100 MB, so upload it from the Data Room in the browser. Empty files, hidden files, Office lock files and partial downloads such as `.tmp`, `.crdownload` and `.part` are skipped.

Uploads only run while the desktop app is open and signed in. A file saved while it's closed waits in the folder and uploads the next time you open the app and sign in.
