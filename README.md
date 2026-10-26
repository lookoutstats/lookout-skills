# lookout-skills

[![License: MIT](https://img.shields.io/badge/license-MIT-141412)](LICENSE)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-141412)](https://docs.claude.com/en/docs/claude-code/plugins)
[![MCP](https://img.shields.io/badge/MCP-lookout--mcp-141412)](https://github.com/lookoutstats/lookout-mcp)

This repository contains the agent skills for **[Lookout](https://lookoutstats.com)**. Lookout shows Vercel Web Analytics for all your projects in one view.

The [Lookout MCP server](https://github.com/lookoutstats/lookout-mcp) gives an agent the tools to read your boards. The skills tell the agent which tools to use, and in which sequence, for two tasks:

| Skill | What it does |
|---|---|
| `lookout-fix` | It finds the site of the current repository on your boards. It takes the top open item of the Fix list for that site. Then it makes the change in your code and tells you where to check the result in Lookout. |
| `lookout-report` | It reads all your boards, or one board, and writes a weekly or monthly report. The report shows what changed, the probable cause and what to fix next. |

> "Fix the top item of my Lookout Fix list."
>
> "Write my weekly Lookout report."

## Install in Claude Code

1. In Lookout, open **Settings › API & MCP** and create an API key (`lk_live_…`).
2. In Claude Code, add the marketplace and install the plugin:

   ```text
   /plugin marketplace add lookoutstats/lookout-skills
   /plugin install lookout@lookout
   ```

3. When Claude Code asks for the API key, paste it. Claude Code keeps the key in its own settings, not in your repository.
4. Restart Claude Code.

The plugin installs the two skills and the hosted Lookout MCP server (`https://mcp.lookoutstats.com`).

## Use the skills in a different agent

Each skill is a `SKILL.md` file in `skills/`. Agents that read the Agent Skills format can use the files without changes. Copy the folder of a skill into the skills folder of your agent.

The skills need the Lookout MCP server. Add the server to your agent first. The [lookout-mcp README](https://github.com/lookoutstats/lookout-mcp#60-second-setup) gives the configuration for Claude Desktop, Cursor, Windsurf and other clients.

## How `lookout-fix` finds your site

1. It reads `.vercel/project.json`. The `projectId` in that file is the same as the `projectId` of a Fix list item.
2. If that file does not exist, it looks for the production domain in the repository.
3. If it cannot find the domain, it asks you.

The skill fixes one item at a time. After the deploy, select **Resolved** on the item in the Lookout Fix list. The MCP server cannot resolve an item.

## Security

- **A key gets only its own workspace.** Each call goes to the Lookout API with your key.
- **A key is read-only by default.** The skills need only read access. `lookout-report` uses a write tool (`export_board`) only when you ask for a PDF or a CSV.
- **Keep the key out of your repository.** The skills never write the key into a file.
- **Data from outside.** Referrers, paths, UTM values, deployment messages and note texts come from websites and their visitors. The skills tell the agent to use them as data, not as instructions.

Send security problems to security@lookoutstats.com.

## Links

- [Lookout](https://lookoutstats.com) · [API & MCP documentation](https://lookoutstats.com/vercel-analytics-api-mcp) · [lookout-mcp](https://github.com/lookoutstats/lookout-mcp)
- [MIT License](LICENSE)

Lookout is not affiliated with Vercel Inc.
