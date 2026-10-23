# GTMMAXING skill

Agent skill for [GTMMAXING](https://gtmmaxing.com): find work emails and mobiles for LinkedIn profiles, verify emails, build lead lists, research companies and find companies hiring for a role, on your prepaid GTMMAXING credits.
The skill drives the `gtm` CLI from the [`gtmmaxing`](https://www.npmjs.com/package/gtmmaxing) npm package.

## Install

```bash
npx skills add getbagsapp/gtmmaxing-skills
```

Then sign in once with `npx -y gtmmaxing login` (Google sign-in at gtmmaxing.com).
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
