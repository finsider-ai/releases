# Connect Finsider to your AI tools

Finsider connects to your AI app through one link, and your Finsider account determines which companies and workspaces you can access.

Pick the section for the tool you use.

- Claude on the web, desktop and mobile, plus Claude Cowork - add a custom connector and sign in. See [Claude](#claude-on-the-web-desktop-mobile-and-cowork).
- ChatGPT - add a custom connector in developer mode and sign in. See [ChatGPT](#chatgpt).
- Claude Code and Codex - run one command and sign in through your browser. See [Claude Code](#claude-code) and [Codex](#codex).
- Cursor and Grok - not supported yet. See [Cursor and Grok](#cursor-and-grok).
- Any agent that can save files on your computer - add documents to a company's Data Room through [Finsider Drop](#finsider-drop).

The same steps with copy buttons are on the Finsider install page at https://home.finsider.ai/agents.

## Connect with the Finsider link

Every app below uses this link.

```text
https://agents.finsider.ai/mcp
```

Each app registers itself with Finsider the first time you connect, so you don't need a client ID, OAuth settings or the Finsider CLI. You sign in with your own Finsider account and approve access once. Hosted sign-in goes live with the next Finsider gateway release, and these steps work from then on.

### Claude on the web, desktop, mobile and Cowork

Claude keeps connectors on your account, so the connector you add once works on the web, in the desktop and mobile apps and in Claude Cowork. Claude Desktop connects this way too and no longer needs a config file.

1. Open [Claude connector settings](https://claude.ai/settings/connectors), or in Claude click **Customize**, then **Connectors**.
2. Click **+**, then **Add custom connector**.
3. Enter `Finsider` as the name and paste the Finsider link as the URL. Leave **Advanced settings** empty, because Claude registers itself. Click **Add**.
4. Click **Connect**, sign in with your Finsider account and approve access.
5. Start a new chat, turn Finsider on for the conversation and ask "List my Finsider workspaces" to check that it answers with your companies.
6. If sign-in fails, remove the connector and add it again so Claude registers fresh. If it still fails, contact Finsider support with the error Claude shows.

On a Team or Enterprise plan, an owner first adds the connector under **Organization settings**, then **Connectors**, and each person then connects their own account.

The connector supplies Finsider's tools but not the bundled workflow, so upload the [diligence skill](../plugins/finsider/skills/diligence/SKILL.md) where your plan allows skills, or paste its workflow into your request.

### ChatGPT

ChatGPT connects through a custom connector in developer mode, which works on Plus, Pro, Business and Enterprise.

1. In ChatGPT, open **Settings**, then **Security and login**, and turn on **Developer mode**. If your workspace doesn't allow it, ask your ChatGPT admin to allow custom apps.
2. Open **Plugins** and click **+**.
3. Enter `Finsider` as the name, a short description and the Finsider link as the MCP URL.
4. Choose **OAuth** when ChatGPT asks how to sign in. You don't need to enter a client ID.
5. Sign in with your Finsider account, approve access and review the tools ChatGPT found.
6. Add Finsider to a new chat and ask "List my Finsider workspaces" to check that it answers with your companies.
7. If sign-in fails, remove the connector and add it again. If it still fails, contact Finsider support with the error ChatGPT shows.

Finsider isn't listed in ChatGPT's public app directory yet, because that listing needs a sign-in change Finsider is still making, so developer mode is the way to connect today. See the [ChatGPT connection guide](https://developers.openai.com/plugins/deploy/connect-chatgpt) for workspace settings.

### Claude Code

1. Run this once in your terminal.

   ```bash
   claude mcp add --transport http finsider https://agents.finsider.ai/mcp
   ```

2. Open Claude Code and type `/mcp`.
3. Choose **finsider**, then sign in with your Finsider account in the browser window that opens.
4. Check that `/mcp` shows Finsider as connected, then ask "List my Finsider workspaces".
5. If sign-in fails, run `claude mcp remove finsider`, repeat step 1 and sign in again.

### Codex

1. Run these two commands in your terminal. The second one opens your browser to sign in.

   ```bash
   codex mcp add finsider --url https://agents.finsider.ai/mcp
   codex mcp login finsider
   ```

2. Sign in with your Finsider account and approve access.
3. Start a new Codex session and check that Finsider appears in the available tools.
4. If sign-in fails, run `codex mcp login finsider` again.

### Cursor and Grok

Finsider doesn't support Cursor or Grok yet. Their sign-in isn't on Finsider's trusted list, so adding the Finsider link in either app won't connect.

## Use Finsider

Ask "Use Finsider to assess diligence readiness for my selected company and period." Finsider gathers evidence before proposing changes. Review each proposed change and its exact preview before you approve it.

If sign-in expires, connect again from your app's connector settings, or run `finsider auth login` if you use the CLI bridge below. Use one connection method per app, so the app doesn't load two Finsider servers.

## Advanced CLI bridge

The Finsider CLI can run a local bridge, `finsider mcp serve`, that your AI app starts on your computer. Most people should use the link above. Use the bridge if you already work in the Finsider CLI or want the Finsider plugin, which bundles the diligence workflow with the tools in Claude Code and Codex.

### Sign in to the CLI

Install Finsider CLI 1.1.0 or newer using the [CLI setup instructions](https://home.finsider.ai/agents), then run these commands.

```bash
finsider --version
finsider auth login
```

Complete sign-in in your browser. The plugin starts `finsider mcp serve` with that signed-in account, so you don't need to run the bridge yourself.

### Claude Code plugin on macOS or Linux

Run these commands in your terminal.

```bash
claude plugin marketplace add finsider-ai/releases
claude plugin install finsider@finsider --scope user
```

Start a new Claude Code session. Use `/mcp` to check the Finsider connection.

### Codex plugin on macOS or Linux

Run these commands in your terminal.

```bash
codex plugin marketplace add finsider-ai/releases
codex plugin add finsider@finsider
```

Start a new Codex session and confirm Finsider appears in the available tools.

These commands use the public Finsider marketplace in this repository. Access to a private Finsider source repository is unnecessary. See the official [Claude Code plugin guide](https://code.claude.com/docs/en/discover-plugins) and [Codex plugin documentation](https://developers.openai.com/plugins/build/plugins) for host settings and workspace policies.

### Windows PowerShell

Use the CLI-generated connection command for your AI client. Automatic marketplace MCP startup on Windows is still being verified.

```powershell
finsider.cmd auth login
# Choose the client you use:
finsider.cmd mcp config codex
finsider.cmd mcp config claude
```

Copy and run the setup command printed for your chosen client once in PowerShell, then start a new client session. The generated command uses the correct executable paths for your Windows installation. Keep this standalone connection separate from the marketplace plugin so a second Finsider MCP server is not enabled. The [diligence workflow](../plugins/finsider/skills/diligence/SKILL.md) is available as a reference alongside the connection.

### Claude Desktop through the CLI bridge

Claude Desktop connects through the [Claude connector](#claude-on-the-web-desktop-mobile-and-cowork) above. Use this route only if you want Claude Desktop to start the local `finsider mcp serve` bridge from its config file instead. Claude Desktop doesn't read your terminal's PATH, so the config needs the full path to the Finsider CLI.

1. Install the CLI and sign in as described in [Sign in to the CLI](#sign-in-to-the-cli).
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

A coming CLI release adds `finsider mcp config claude-desktop`, which prints this snippet with your paths already filled in. Until that release ships, copy the paths by hand as above. Use one connection method, so don't also add the Claude connector while this bridge is set up.

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
