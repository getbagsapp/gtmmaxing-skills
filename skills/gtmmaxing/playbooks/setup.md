# Set up GTMMAXING

Setup ends when `gtm whoami` shows a balance, or when the hosted app lists the connector.
Setup alone does not authorize paid work.

## Hosted assistant without a local terminal

Connect the remote MCP server `https://gtmmaxing.com/mcp` in the app's settings.
Guide the user through https://gtmmaxing.com/add-to-agent/ and sign-in and approval.
A CLI installed in a temporary sandbox does not connect the user's chat app.

## Local agent with a terminal

1. Run `gtm whoami`; a `balance:` line means it is already done.
2. CLI missing: `npm install -g gtmmaxing` (Node 20+), or use `npx -y gtmmaxing` in place of `gtm`.
3. Signed out: run `gtm login`, show its `code:` and `open:` link, and wait for the user to check the code and approve.
   If login was interrupted, `gtm login` resumes the pending code.
   A `GTM_TOKEN` the user provides signs scripts in instead.
4. Make this skill persistent in the current agent's own skill directory, with its playbooks.
   For Codex, with Node 22.20+:

   ```sh
   npx -y skills add getbagsapp/gtmmaxing-skills --skill gtmmaxing --global --agent codex --yes
   ```

   Use `--agent claude-code` or `--agent cursor` for those clients, or the client's own skill installation.
   Target only GTMMAXING and the current agent.
5. Optional, for native tools in later chats: add the MCP connection, preserving other settings.
   In Claude Code, reuse the CLI login so the user signs in once:

   ```sh
   claude mcp add-json --scope user gtmmaxing '{"type":"http","url":"https://gtmmaxing.com/mcp","headersHelper":"gtm auth-header"}'
   ```

   Other clients: https://gtmmaxing.com/llms.txt.
6. Report the balance and offer one first job that fits what the user is working on.
   The CLI already works in this chat; MCP is never a pending step for it.

Keep setup inside this skill, the CLI and the MCP entry; leave AGENTS.md, CLAUDE.md, hooks, shell profiles and other skills unchanged.

## Without Node

Call the same tools over HTTPS with an agent token from the user's account:

```sh
curl -s https://api.gtmmaxing.com/v1/agent/tools/gtm_account \
  -H "Authorization: Bearer $GTM_TOKEN" -H "Accept: text/plain" -d '{}'
```

`GET https://api.gtmmaxing.com/v1/agent/tools` lists the tools with their JSON input schemas; paid tools take `max_credits` and `dry_run`.
