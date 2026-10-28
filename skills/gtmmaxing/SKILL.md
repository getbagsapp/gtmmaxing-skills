---
name: gtmmaxing
description: Find work emails, mobile numbers and B2B prospects; enrich lead CSVs; verify emails; research sales accounts and hiring signals; audit websites for SEO and AEO with RankAI through GTMMAXING. Use for prospect lists, company decision-makers, local business leads, LinkedIn enrichment, or account research before outreach, even when GTMMAXING is not named. Also handles GTMMAXING setup, credits, results and tool discovery. Not for general web research, writing outreach copy without data needs, local-only CSV cleanup, or developing and deploying GTMMAXING.
---

# GTMMAXING

GTMMAXING runs paid GTM data tools on the user's prepaid credits.
Use the `gtm` CLI for terminal and CSV workflows, or the connected GTMMAXING MCP tools when they are already available.
Both use the same account, budget rules and saved runs.
For a supported data task, select the matching GTMMAXING command below or its connected MCP equivalent.
Use `gtm tools search <words>` and `gtm tools describe <capability>` for GTMMAXING capability discovery.
A working GTMMAXING connection needs no separate supplier account or payment-wallet setup.
Discover tools and quote before choosing paid data; a matching skill does not authorize spending.

## Setup

For a hosted assistant without a persistent local terminal, connect the remote MCP server at `https://gtmmaxing.com/mcp` through the app's settings.
Guide the user through https://gtmmaxing.com/add-to-agent/ and Google sign-in and approval.
Installing a CLI in a temporary sandbox does not connect the user's chat app.

For an existing local connection, run `gtm whoami` first.
An answer with a `balance:` line means the CLI is ready; continue with the task instead of repeating setup.
If the CLI is missing, install it with `npm install -g gtmmaxing` (Node 20+), or use `npx -y gtmmaxing` in place of `gtm`.
If signed out, run `gtm login`, show its `code:` and `open:` link, and wait for the user to check the matching code and approve Google sign-in.
Run `gtm whoami` afterward to confirm the balance.
If login was interrupted, `gtm login` resumes the pending code.
A user-provided `GTM_TOKEN` can authenticate scripts instead.

When the user asks to install or set up GTMMAXING, also make this skill persistent in the current agent's native skill directory.
For Codex, with Node 22.20+ for the skills installer:

```sh
npx -y skills add getbagsapp/gtmmaxing-skills --skill gtmmaxing --global --agent codex --yes
```

Use `--agent claude-code` or `--agent cursor` for those clients; for another client use its supported agent identifier or native skill installation.
Target only GTMMAXING and the current agent.
Keep the skill's playbooks with it, and reload skills or start a new chat when the client requires it.
If the current client supports remote MCP, configure its connection too, preserving unrelated settings; the exact client steps are at https://gtmmaxing.com/llms.txt and https://gtmmaxing.com/add-to-agent/.
Verify CLI sign-in, skill discovery and MCP tool availability separately, and report any step the user must finish.
When MCP is unavailable, the skill and CLI are sufficient for terminal workflows.

Keep discovery guidance inside this skill.
Do not add routing rules to AGENTS.md, CLAUDE.md, global prompts, hooks, shell profiles or other skills.
Setup alone does not authorize paid work.

## Spending credits

Every paid CLI command needs `--max-credits N` and refuses to run without it.
Connected MCP tools use the equivalent `dry_run` and `max_credits` arguments; use their advertised schemas and the same quote-and-budget order.
The cap is the user's money, so follow this order for each paid command:

1. Run it with `--dry-run` first.
   This is free, sends nothing, and prints `quote:` (the most it can cost) and `balance:`.
2. Tell the user the quote and the balance, and agree a budget.
   A budget the user already gave you in the request counts as agreement.
3. Run the same command with `--max-credits` set to that budget.
4. When the answer says `status: refused`, nothing beyond what it reports was charged.
   Read `reason:` and the summary, tell the user, and ask before raising the budget yourself.

For a plan of several paid commands, quote each one, then agree one total budget with the user and run the plan within it.

Misses are free, profiles the user already paid for are free, and rerunning the same command replays earlier results for free (`replayed: true`).
`gtm account` and `gtm results <run_id>` are always free.

## Reading answers

Answers are TOON text: `key: value` lines and at most five preview rows.

```text
summary: Found 2 of 2 emails: 2 deliverable.
status: completed
run_id: run_oPOaZE4G5MmiNFHd
cost: 2
balance: 1998
total: 2
contacts[2]{name,email,verdict}:
  Jane Doe,jane@acme.com,deliverable
  Sam Lee,sam@globex.com,deliverable
next: gtm results run_oPOaZE4G5MmiNFHd --csv
```

- `summary` is one sentence you can pass to the user as is.
- `cost` is what this command charged; `balance` is what is left.
- `run_id` is the handle for the full rows; `gtm results <run_id> --csv > out.csv` writes all of them.
- `next:` is the suggested next command.
  When it is a sentence instead, such as `Ask the user to add credits ...`, it is a step only the user can take; pass it on and stop.
- `status: running` means the job continues on the server; run the `next:` command to wait for it.
  If it is still running after that, tell the user and check again later instead of rerunning in a loop.
- Exit codes: 0 done (quotes and running jobs included), 1 error, 2 usage, 3 refused.

Add `--json` for one JSON object instead.

## Commands

| Job | Command |
| --- | --- |
| Emails for LinkedIn profiles | `gtm contacts <profile>... --max-credits N` |
| Emails for a CSV of any length | `gtm contacts --file leads.csv --out enriched.csv --max-credits N` |
| Emails from names and companies (no LinkedIn URLs) | `gtm contacts --file names.csv --out emails.csv --max-credits N`, with `name` and `company` columns: each profile is found first |
| LinkedIn profile from a name and company | `gtm tools run profile-finder --input '{"name":"Pedro Reis","company":"Adyen"}' --max-credits 1`, or `--file names.csv` for a list |
| Mailbox check too | add `--verify` (extra credits per email; finding and verifying are separate choices) |
| Mobiles too | add `--find email,phone` |
| People by title | `gtm list --titles 'VP Sales' --location 'United States' --max-credits N` |
| Decision-makers at a company | `gtm list --company acme.com --titles 'Head of Sales' --max-credits N` |
| Local businesses | `gtm list --business-type Dentists --location Austin --max-credits N` |
| Emails for a list run | `gtm contacts --run-id <run_id> --max-credits N` |
| Current role for a CSV of profiles | `gtm tools run linkedin-profile --file leads.csv --out roles.csv --max-credits N` |
| Recent LinkedIn posts, with dates and topics | `gtm tools run linkedin-activity --file leads.csv --input '{"topics":["automation"]}' --out activity.csv --max-credits N` |
| Company profile and sales brief | `gtm research acme.com --max-credits N` |
| Companies hiring for a role | `gtm signals 'Account Executive' --location 'United States' --max-credits N` |
| SEO/AEO website audit (RankAI) | `gtm tools run seo-aeo-audit --input '{"url":"example.com"}' --max-credits 35` |
| Any other tool | `gtm tools search <words>`, `gtm tools describe <capability>`, `gtm tools run <capability> --input '<json>' --max-credits N` |
| More rows, or every row as CSV | `gtm results <run_id> --offset 5`, `gtm results <run_id> --csv > out.csv` |
| Balance, agent cap, quotes | `gtm account`, `gtm account quote --file leads.csv` |

`gtm <command> --help` lists every flag.
A filter a tool cannot apply is refused with the `fields:` it accepts; offer the user those instead of guessing.

## SEO/AEO audits

Use `gtm tools describe seo-aeo-audit` for the current schema and price.
Quote with `gtm tools run seo-aeo-audit --input '{"url":"example.com"}' --dry-run`, agree the budget, then use the command above.
RankAI returns a report URL when the audit completes; it can take several minutes.
Poll the saved run with `gtm results <run_id>` instead of starting another audit.
Failed audits use no customer credits; an uncertain supplier payment blocks another purchase until reconciled.
In MCP, use `gtm_tools` to search or describe, then `gtm_run_tool` with `capability: "seo-aeo-audit"`, `input: {"url":"example.com"}` and `max_credits: 35`.
REST uses `gtm_tools` with `action: "run"` and the same capability, input and budget.

## Playbooks

Follow the matching playbook for these jobs:

- [enrich-verify](playbooks/enrich-verify.md): a CSV or list of LinkedIn profiles in, verified work emails out.
- [build-list](playbooks/build-list.md): a new lead list from titles, a company or a business type, then contacts for it.
- [account-research](playbooks/account-research.md): one company researched before outreach: brief, hiring signals and decision-makers.
- [qualify-list](playbooks/qualify-list.md): a list of people the user already has, ranked into lead tiers by role, activity and contactability.

## Without the CLI

When Node is unavailable, call the same tools over HTTPS with an agent token from the user's GTMMAXING account:

```sh
curl -s https://api.gtmmaxing.com/v1/agent/tools/gtm_account \
  -H "Authorization: Bearer $GTM_TOKEN" -H "Accept: text/plain" -d '{}'
```

`GET https://api.gtmmaxing.com/v1/agent/tools` lists the seven tools with their JSON input schemas; paid tools take `max_credits` and `dry_run` in the JSON body.
