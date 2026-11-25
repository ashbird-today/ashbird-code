<p align="center"><img src="https://raw.githubusercontent.com/ashbird-today/ashbird-code/main/media/ashbird-hero.gif" alt="Ashbird" width="800"></p>

# Ashbird Code

**An AI coding agent that does more with fewer tokens. European, and your code stays yours.**

Ashbird Code is an agentic coding chat for VS Code, Cursor, Windsurf and VSCodium. It reads and edits your files, runs your tests, and doesn't call a task done until the checks pass.

- **Fewer tokens, faster runs.** In our own benchmark against Cline on the same model, Ashbird Code used about 4.6x fewer tokens and finished about 2x faster, with the same task quality.
- **Pay for what you use.** Credits are charged at cost. You see the price of every run, and you can set a credit cap.
- **European and private.** Calls go through the Ashbird gateway to a European model provider. Tools run on your machine. Older conversation context is encrypted with a key only you hold.
- **Bring your own key** if you prefer your own model provider.

<!-- Product screenshots go here. -->

Get started: install, open the Ashbird icon in the sidebar, and sign in. New accounts get starter credits. See [ashbird.today/pricing](https://ashbird.today/pricing).

## What it does

- Chat with a model that can read and edit files in your workspace.
- The agent runs terminal commands and checks, and asks before it makes changes unless you allow them.
- Each agent run has checkpoints, so you can undo its edits.
- Checks (tests or a compiler) run before the agent can declare a task done.
- Explain a selection, or fix problems in the current file, from the editor menu.
- Add MCP servers to give the agent more tools.

## Sign in

Run **Ashbird: Sign In** from the Command Palette, or open the Ashbird sidebar. Sign-in opens your browser. Sign out with **Ashbird: Sign Out**.

## Pricing

Usage is metered in credits. See https://ashbird.today/pricing for plans and credit prices. The balance and the cost of each run are shown in the sidebar.

## Privacy

- Your code goes only to the model call, through the Ashbird gateway with a European model provider.
- Tools run locally on your machine. File edits, terminal commands and checks never leave your machine except as their output in the model call.
- Sign-in tokens are stored in VS Code SecretStorage.
- Telemetry follows VS Code's telemetry setting and the `ashbird.telemetry` setting. Both must be on for any usage data to be sent. Telemetry never includes code, paths or prompts.

## Settings

| Setting | Default | Description |
| --- | --- | --- |
| `ashbird.apiBase` | `https://api.ashbird.today` | Ashbird API origin. |
| `ashbird.scope` | `lite` | How much earlier work the model sees. `lite` is the cheapest; `budget` keeps fuller recent history; `smart` and `long` show more and cost more. Also set with **Ashbird: Choose scope**. |
| `ashbird.autoApproveEdits` | `false` | Apply file edits without asking first. |
| `ashbird.allowCommands` | `[]` | Commands the agent may run without asking. Each entry is a command prefix, for example `npm test`. |
| `ashbird.denyCommands` | `[]` | Commands the agent may never run. Each entry is a command prefix, for example `rm -rf`. |
| `ashbird.stepCap` | `50` | Maximum number of agent steps in one run (1 to 500). |
| `ashbird.runCreditCap` | empty | Stop a run once it has spent this many credits. Empty or 0 means no cap. |
| `ashbird.doneGate` | `true` | Run the project's checks before the agent may declare a task done. |
| `ashbird.testCommand` | empty | Command the done check runs, for example `npm test`. Empty means detect it automatically. |
| `ashbird.toolFormat` | `native` | How tools are sent to the model. `native` uses tool calling. `text` uses plain-text tool blocks. |
| `ashbird.sendEditorContext` | `true` | Include the open file and selection from the editor in chat context. |
| `ashbird.telemetry` | `false` | Send anonymous usage counts and durations. Only used when VS Code telemetry is also on. |
| `ashbird.mcpServers` | `{}` | MCP servers available in every workspace, in the same shape as `.mcp.json` `mcpServers`. |
| `ashbird.researchConnector` | `false` | Add the built-in Ashbird Research MCP server. Set it from the MCP panel (Connect), or in user settings only; workspace settings cannot enable it. |
| `ashbird.models.head` | `ashbird` | Model for the main agent: Ask, Questions, Plan, Agent and the Big Task head. `ashbird` or `byok` (your own key). |
| `ashbird.models.worker` | `ashbird` | Model for Big Task workers. `ashbird` or `byok` (your own key). |
| `ashbird.bigTask` | `false` | Show the Big Task mode. |
| `ashbird.bigTaskWorkers` | `3` | Most Big Task workers running at once (1 to 5). |

## Models

Two roles choose a model each. Set them in Settings, or in the Options panel under Models.

- **Head** (`ashbird.models.head`) runs Ask, Questions, Plan, Agent and the Big Task head. The head plans and reviews. It has read-only tools.
- **Worker** (`ashbird.models.worker`) runs Big Task workers, which make the edits.

Each role is `ashbird` (the Ashbird model) or `byok` (your own key). Both default to `ashbird`. The header shows "Head: Your key · Workers: Ashbird" when they differ.

If a role is set to `byok` and no usable key is set, the run uses the Ashbird model and shows "No key set, using Ashbird" once. The run does not fail.

## Big Task mode

Big Task is a fifth mode, `/mode bigtask`. It is off until you turn on `ashbird.bigTask` or the "Big Task mode" toggle in Options. It is not available in Ashbird Unli or Ashbird Save. It works like a small team: the head plans, workers edit, and the head fixes what workers cannot.

1. Give it a plan (from Plan mode, or typed) or a saved brief in `.ashbird/quest/*.md`.
2. The head writes the steps. Each step names its files and a check. Every name that crosses steps has one owner step, listed in a contract that every worker sees. The plan also has one whole-task check.
3. Before each wave, the step checks are dry-run. A broken command, or a check that already passes, goes back to the head once.
4. Review the plan card, then choose Start, Edit plan or Cancel.
5. Each step runs in its own worker with its step, the contract, its files, and your project instructions and memory. Workers have no chat history. They can write only to their step's files, and they have no run tool.
6. The head checks each step's diff and check. A review that names a defect always sends the step back.
7. If a step fails, it is escalated. Attempt 1 is the worker. Attempt 2 is the same worker, given the failure. Attempt 3 is the head doing the step itself. Steps that depend on a failed step are skipped.
8. After the last wave, the head runs the whole-task check and fixes the seams. It can write only to the files the run touched and the contract's files, in at most 2 rounds.
9. The head writes the report: who did each step, each escalation, and the final check result.

Scratch notes for each worker are kept in the extension's storage (not your workspace) and are deleted after 7 days. **Stop** cancels every worker. **Undo last run** reverts every step of the run. `ashbird.runCreditCap` stops the whole run and keeps the finished steps. Integrate and escalation calls are head calls and count toward that cap.

Big Task uses more calls than Agent mode. Its value on the Ashbird model is the per-step context and checks. A stronger head model helps most when set to your own key.

## Commands

| Command | Description |
| --- | --- |
| Ashbird: Sign In | Sign in with your Ashbird account. |
| Ashbird: Sign Out | Sign out and revoke the stored token. |
| Ashbird: Paste an API Key | Use an API key instead of signing in. |
| Ashbird: Refresh Balance | Reload your credit balance. |
| Ashbird: New Chat | Start a new chat. Keyboard shortcut: `Ctrl+Shift+L` (`Cmd+Shift+L` on macOS). |
| Ashbird: Explain Selection | Explain the selected code in the chat. |
| Ashbird: Fix Problems in File | Ask the agent to fix the problems reported for the current file. |
| Ashbird: Report a problem | Send a problem report. |
| Ashbird: Restart MCP servers | Restart the configured MCP servers. |
| Ashbird: Reset MCP server approvals for this workspace | Ask again before each MCP server is used in this workspace. |
| Ashbird: Edit user instructions | Open (or create) your personal instructions file, `~/.ashbird/ASHBIRD.md`. |

## MCP servers

The **MCP** button in the chat header shows each MCP server, whether it is running and how many tools it adds. From there you can restart the servers, open the workspace list (`.ashbird/mcp.json`) or your own list (the `ashbird.mcpServers` setting), and reset this workspace's approvals. **Add server** takes a name and either a command (for example `npx -y @modelcontextprotocol/server-filesystem .`) or an http(s) URL, and saves it for this workspace or for all workspaces; each server has a **Remove** button.

### Ashbird Research

Ashbird Research is the first row at the top of the MCP panel. To add it:

1. Open the **MCP** button in the chat header.
2. Press **Connect**, or use the `vscode://ashbird.ashbird-code/research/connect` link, or the "Add to Ashbird Code" button on ashbird.today/mcp.
3. Confirm in the browser. There is no token to copy.

Research runs are paid from your Ashbird credits. **Disconnect** in the same row revokes the token on the server and deletes it locally. Signing out also revokes it.

For security, workspace settings cannot enable Research. Only your user (global) settings can, so a repository's `.vscode/settings.json` cannot turn it on.

## Customising Ashbird Code

Everything here is a local file on your machine. Nothing is uploaded except what a run sends in a model call.

### Skills

A skill is a folder with a `SKILL.md` file. The agent sees a one-line description of each skill and loads the full file when it fits the task. Skills are looked up in this order: `.ashbird/skills/<name>/`, `.claude/skills/<name>/` in the workspace, then `~/.ashbird/skills/<name>/` and `~/.claude/skills/<name>/`. If two skills have the same name, the higher one wins. Changes apply without a reload.

```markdown
---
name: release-notes
description: Write release notes from the git log since the last tag.
---

Group the commits by type. Keep each note to one line.
```

Add `disable-model-invocation: true` to the frontmatter to hide a skill from the agent. You can still run it yourself as `/<name>`.

### User instructions

Put your own instructions in `~/.ashbird/ASHBIRD.md`. Run "Ashbird: Edit user instructions" to open it. The file is sent before the project instructions (`ASHBIRD.md`, `AGENTS.md` or `CLAUDE.md` at the workspace root). Together they share 8 KB: your file may use up to 3 KB, and the project gets the rest.

### Custom commands

A custom command is a Markdown file in `<workspace>/.ashbird/commands/` or `~/.ashbird/commands/`. The workspace version wins. The file name becomes the command, so `review.md` becomes `/review`.

```markdown
---
description: Review the current file for bugs.
mode: ask
---

Review $ARGUMENTS for bugs. List each one with its line number.
```

The body is the prompt. `$ARGUMENTS` is replaced by the text you type after the command. If the body has no `$ARGUMENTS`, the text is added at the end. `mode` can be `ask`, `agent`, `plan` or `questions`. Built-in commands win over custom ones with the same name. `/help` lists your custom commands and skills.

## Licence

Proprietary. Free to install and use with an Ashbird account; see [LICENSE.md](LICENSE.md).

## Feedback and issues

Found a bug or have an idea? [Open an issue](https://github.com/ashbird-today/ashbird-code/issues/new/choose). For security problems, see [SECURITY.md](SECURITY.md).
