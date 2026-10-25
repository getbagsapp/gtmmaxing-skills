---
name: gtmmaxing
description: GTMMAXING through the gtm CLI. Use to find work emails or mobiles for LinkedIn profiles or a CSV of leads, verify emails, build lead lists (people by title, decision-makers at a company, local businesses), research a company, or find companies hiring for a role.
---

# GTMMAXING

GTMMAXING runs paid GTM data tools on the user's prepaid credits.
The `gtm` CLI is the interface: every command prints one short answer, and the full data stays on the server until you ask for it.

## Setup

If you cannot run terminal commands, guide the user to https://gtmmaxing.com/add-to-agent/ for their app’s connector steps.
Do not claim you installed a connector yourself.
Connecting requires a GTMMAXING account and the user’s Google sign-in and approval.
Setup alone does not authorize paid work.

1. Run `gtm whoami`.
   An answer with a `balance:` line means you are signed in; go to the task.
2. If `gtm` is missing, install it with `npm install -g gtmmaxing` (Node 20 or newer), or prefix every command with `npx -y gtmmaxing` instead of `gtm`.
3. If `gtm whoami` says `reason: not_signed_in`, run `gtm login`.
   It prints a `code:` and an `open:` link, opens the browser when it can, and waits.
   Show the user both lines and ask them to check that the page shows the same code and click Approve.
   If your shell stops the command before they approve, run `gtm login` again: it resumes the same code.
   With a token from the user's GTMMAXING account instead, set `GTM_TOKEN` and skip the login.

Setup is done when `gtm whoami` prints a balance.

## Spending credits

Every paid command needs `--max-credits N` and refuses to run without it.
The cap is the user's money, so follow this order for each paid command:

1. Run it with `--dry-run` first.
   This is free, sends nothing, and prints `quote:` (the most it can cost) and `balance:`.
2. Tell the user the quote and the balance, and agree a budget.
   A budget the user already gave you in the request counts as agreement.
3. Run the same command with `--max-credits` set to that budget.
4. When the answer says `status: refused`, nothing beyond what it reports was charged.
   Read `reason:` and the summary, tell the user, and ask before raising the budget yourself.

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
| Emails for LinkedIn profiles | `gtm contacts <profile>... --verify --max-credits N` |
| Emails for a CSV of any length | `gtm contacts --file leads.csv --verify --out enriched.csv --max-credits N` |
| Mobiles too | add `--find email,phone` |
| People by title | `gtm list --titles 'VP Sales' --location 'United States' --max-credits N` |
| Decision-makers at a company | `gtm list --company acme.com --titles 'Head of Sales' --max-credits N` |
| Local businesses | `gtm list --business-type Dentists --location Austin --max-credits N` |
| Emails for a list run | `gtm contacts --run-id <run_id> --verify --max-credits N` |
| Company profile and sales brief | `gtm research acme.com --max-credits N` |
| Companies hiring for a role | `gtm signals 'Account Executive' --location 'United States' --max-credits N` |
| Any other tool | `gtm tools search <words>`, `gtm tools describe <capability>`, `gtm tools run <capability> --input '<json>' --max-credits N` |
| More rows, or every row as CSV | `gtm results <run_id> --offset 5`, `gtm results <run_id> --csv > out.csv` |
| Balance, agent cap, quotes | `gtm account`, `gtm account quote --file leads.csv` |

`gtm <command> --help` lists every flag.
A filter a tool cannot apply is refused with the `fields:` it accepts; offer the user those instead of guessing.

## Playbooks

Follow the matching playbook for these jobs:

- [enrich-verify](playbooks/enrich-verify.md): a CSV or list of LinkedIn profiles in, verified work emails out.
- [build-list](playbooks/build-list.md): a new lead list from titles, a company or a business type, then contacts for it.
- [account-research](playbooks/account-research.md): one company researched before outreach: brief, hiring signals and decision-makers.

## Without the CLI

When Node is unavailable, call the same tools over HTTPS with an agent token from the user's GTMMAXING account:

```sh
curl -s https://api.gtmmaxing.com/v1/agent/tools/gtm_account \
  -H "Authorization: Bearer $GTM_TOKEN" -H "Accept: text/plain" -d '{}'
```

`GET https://api.gtmmaxing.com/v1/agent/tools` lists the seven tools with their JSON input schemas; paid tools take `max_credits` and `dry_run` in the JSON body.
