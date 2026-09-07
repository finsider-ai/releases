# Finsider

Financial diligence through your authenticated Finsider workspace: assess
engagement readiness, inspect evidence, ask Fin, approve specific changes,
verify their results, and prepare reports for professional review.

This package includes Codex and Claude Code manifests, the `finsider mcp serve`
bridge configuration, and the `diligence` skill. Install the CLI release that
includes the bridge and run `finsider auth login` before installing this plugin.
The bridge uses the signed-in Finsider account to call the hosted gateway.
The remote connector endpoint is `https://agents.finsider.ai/mcp`; the package
contains no credentials. The conversation uses the model
selected in the host. Finsider's `ask_fin` tool invokes its separately configured
server-side analyst.

This is a distribution candidate. Production readiness and desktop host checks must pass before customer rollout.
Remote cloud connectors additionally need host OAuth registration. See the public
[client setup](https://github.com/finsider-ai/releases/blob/main/docs/plugin-setup.md)
for installation and supported connection methods.

After installing and connecting, ask: “Use Finsider to assess diligence
readiness for my workspace for January through June 2026.” Specify the company
and period when you know them. Finsider gathers evidence before proposing any
changes, and each mutation requires approval of the server's concrete preview.
