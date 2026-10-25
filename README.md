# GTMMAXING skill

Agent skill for [GTMMAXING](https://gtmmaxing.com): find work emails and mobiles for LinkedIn profiles, verify emails, build lead lists, research companies, find companies hiring for a role and audit websites for SEO/AEO with RankAI, on your prepaid GTMMAXING credits.
The skill drives the `gtm` CLI from the [`gtmmaxing`](https://www.npmjs.com/package/gtmmaxing) npm package.

## Install

For full setup, paste this into your agent:

```text
Install GTMMAXING's CLI and skill, connect MCP if supported, and help me sign in: https://gtmmaxing.com/llms.txt
```

Local agents install the CLI and their own native skill, then add MCP where supported.
Hosted chat apps connect through their remote MCP settings instead.
Setup instructions do not add routing rules to AGENTS.md, CLAUDE.md or other skills.

For manual local setup with Node.js 22.20+, this example targets Codex:

```bash
npm install -g gtmmaxing
npx -y skills add getbagsapp/gtmmaxing-skills --skill gtmmaxing --global --agent codex --yes
gtm login
gtm whoami
```

Use `--agent claude-code` or `--agent cursor` for those clients, and reload skills or start a new chat when needed.
MCP connection steps are at https://gtmmaxing.com/add-to-agent/.
Every paid command supports a free quote and refuses to spend more than its `--max-credits` limit.

## Contents

- `skills/gtmmaxing/SKILL.md` - the skill, also served at https://gtmmaxing.com/skill.md
- `skills/gtmmaxing/playbooks/` - enrich and verify, build a list, account research

Remote MCP connector for chat apps: `https://gtmmaxing.com/mcp`.

## Links

- Website: https://gtmmaxing.com
- Privacy: https://www.getbags.app/privacy
- Terms: https://www.getbags.app/legal
- Support: vedant@getbags.app
