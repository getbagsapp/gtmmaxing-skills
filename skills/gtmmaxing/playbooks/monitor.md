# Monitor accounts, people and topics

A watch checks on a fixed schedule and charges only confirmed changes; the first check is a free baseline.

| The user wants to know when | Create with | Checked | Charge |
| --- | --- | --- | --- |
| A company hires for a role | `--hiring SDR` | weekly | 5 credits per new post |
| A company raises money | `--signal funding` | daily | 5 credits per funding round |
| Headcount moves | `--signal headcount` | monthly | 15 credits per company with a change |
| Tools are added or dropped | `--signal tech-stack` | monthly | 15 credits per company with changes in a check |
| A past champion changes job | `--signal job-change` | monthly | 40 credits per person changed |
| People ask for something on X or Reddit | `--topic 'CRM for agencies'` | weekly | 5 credits per qualifying post |

Hiring rules are below; for any other signal, read its section in [monitor-signals](monitor-signals.md) first.

A watch authorizes future spending, so agree a monthly credit limit after the free quote.
An existing authorization counts only when it explicitly covers recurring monthly spending.
A one-time prospecting budget does not authorize a recurring watch.

## Quote and create

A hiring watch takes one role and at most 200 company domains, with at most 500 watched entries per account.
Schedules are fixed and have no cadence option.
A CSV needs a `domain` column, or an explicit `--column`.

```sh
gtm watch create --name "Sales hiring" --file companies.csv --hiring SDR --dry-run
```

Tell the user the balance, remaining agent cap, price per change, worst case per check and worst case per month from the quote.
The quote reserves nothing and creates no watch.
The `quote:` number is the worst case for one check, not the price and not a monthly bill: it covers every possible returned post, rather than predicting how many new posts will appear.
The summary also gives the most a month could cost, which is that figure times the checks in a month, and the user's own monthly limit caps it, so no month costs more than the limit.
Explain that the monthly bill is variable: each reported change has the price in the table, while baseline, empty checks and posts already seen cost zero.
Obtain a monthly limit before creation; it must be at least the worst case per check, or a worst-case check cannot run and the watch pauses.

```sh
gtm watch create --name "Sales hiring" --file companies.csv --hiring SDR --max-credits N --yes
```

The quote's `next:` line is this create command with the quote's own settings and the smallest workable limit already filled in; change `--max-credits` to the limit the user agreed before running it.
The CLI requests another free quote immediately before creation and binds creation to it.
`--yes` confirms the agreed monthly limit without an interactive prompt.
The first check records baseline rows as current signals and charges zero; it runs at the next top of the hour, not at creation.
If connected through MCP, call `gtm_watch` with `action: "create"`, `name`, `companies`, `role` and `dry_run: true`, then repeat the input with the agreed `max_credits` and `quote_id` from the quote's `run_id`.

## Read changes

```sh
gtm watch list
gtm watch show watch_example
gtm watch changes --watch watch_example --new
gtm watch changes --watch watch_example --since 0 --csv
```

`--new` saves a polling cursor under `GTM_CONFIG_DIR`, scoped to the server, credential and watch.
It advances only across returned rows; follow the next command to read additional pages.
Omit `--watch` for all watches in the account.
CSV exports full evidence URLs and titles; compact answers may shorten cells.
Polling is free and does not trigger an unscheduled supplier check.
The MCP equivalent is `gtm_watch` with `action: "changes"`, optional `watch_id` and `since` from the prior answer's `next.args.since`.
Use the evidence as a reason to investigate a company, then ask before starting separate paid contact research.

## Pause and resume

```sh
gtm watch pause watch_example
gtm watch resume watch_example
gtm watch delete watch_example
```

Showing a paused or stopped watch says why in words (paused by you, monthly limit reached, credits ran out, agent spending cap reached, supplier allowance reached, three checks failed in a row, agent access revoked or agent name no longer matches) and that no check is scheduled, and names its companies or topic.
The watch id goes after `show`, `pause`, `resume` or `delete`; `--watch ID` is accepted there too.
Credits, agent-cap, supplier-allowance and three-failure pauses require manual resume.
Only a monthly-credit-limit pause restarts automatically in a new UTC calendar month.
A revoked agent or stopped ENS identity permanently stops the watch.
A check already running must finish before a manual lifecycle action succeeds.
Changes are read by polling or delivered to a signed webhook; email delivery is not offered.

## Signed webhook delivery

The CLI help for watches lists the five webhook commands.

Use `gtm watch webhook_set WATCH_ID --url https://hooks.acme.com/signals` to configure one HTTPS receiver.
The signing secret is returned once; store it privately and never copy it into logs or shared artifacts.
Use `gtm watch webhook_show WATCH_ID`, `gtm watch webhook_rotate WATCH_ID`, `gtm watch webhook_test WATCH_ID` or `gtm watch webhook_remove WATCH_ID` to manage it.
MCP and REST use `gtm_watch` actions `webhook_set`, `webhook_show`, `webhook_rotate`, `webhook_test` and `webhook_remove`, with `watch_id` and an optional `url` for set.
Delivery is free and independent of check charging, with stable IDs, timestamped HMAC signatures and bounded retries.
If the watch reports `webhook failing`, fix the receiver and set its URL or rotate the secret to resume delivery.
