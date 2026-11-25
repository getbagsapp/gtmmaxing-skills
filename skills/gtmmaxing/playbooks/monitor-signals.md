# Rules for each watch signal

Read the one section for the chosen signal; quoting, creating, reading and pausing are the same for all and are in [monitor](monitor.md).

## Monthly company changes

Use `--signal headcount` or `--signal tech-stack` instead of `--hiring ROLE`.
MCP and REST use `gtm_watch` with `signal: "headcount"` or `signal: "tech-stack"` and the same quote and create flow.
These watches check once per calendar month on the same UTC day, clamped for shorter months, with no customer cadence option.
Headcount costs 15 credits per reported change, which is one per company per check.
Tech stack costs 15 credits per company with changes in a check, however many technologies were added or dropped, and every one is still listed.
The first baseline and empty checks are free.
A numeric headcount move must reach both 10 percent and five people relative to the last reported value; a supplier employee-size band move also counts.
Unknown or missing headcounts never imply a reduction.
Technology names are case-normalized, and both additions and drops require two consecutive successful checks before reporting.
A single flicker resets confirmation without repeating a change; a confirmed re-addition after a confirmed drop is a new change.
Technology reads cover the supplier's first 100 results, so changes describe detected technologies rather than a complete inventory.
Supplier allowance is bounded separately at one worst-case company check per watched company per UTC month, including free baselines, with manual resume after a supplier-limit pause.

```sh
gtm watch create --name "Growth" --company acme.com --signal headcount --dry-run
gtm watch create --name "Growth" --company acme.com --signal headcount --max-credits N --yes
gtm watch create --name "Stack" --company acme.com --signal tech-stack --max-credits N --yes
```

## Funding alerts

Use `--signal funding` to be told when a watched company announces a funding round.
MCP and REST use `gtm_watch` with `signal: "funding"` and the same quote and create flow; no role is needed.
Funding watches are checked daily from one shared read of the funding feed, so the number of watches does not change what a check costs.
Each new round costs 5 credits per alert; the baseline and empty checks are free.
Rounds already announced and in the feed when the watch is created are the free baseline, and a round is never reported twice.
A round belongs to a company only when its confirmed website equals a watched domain, or, when no website is known, its whole normalised name equals exactly one watched domain's name.
"Acme Robotics" never matches acme.com, and a company the directory places at another website never matches.
A monthly credit limit is mandatory, and pauses and resumes follow the other signals.
Treat a round as a reason to reach out, not as confirmed buying intent.

```sh
gtm watch create --name "Rounds" --file companies.csv --signal funding --dry-run
gtm watch create --name "Rounds" --file companies.csv --signal funding --max-credits N --yes
```

## Topic watches

Use a topic watch when the user wants weekly monitoring of people asking for something on X, Reddit or both, rather than named companies.
Give `--topic` a buyer-intent phrase, such as "CRM for agencies", and choose `--source x`, `--source reddit` or `--source both` (the default).
MCP and REST use `gtm_watch` with `signal: "topic"`, `topic` and `sources`, and the same quote and create flow; there are no companies or role.
The schedule is weekly and cannot be changed, and each check searches back past the previous check, with a margin so a late check misses nothing, using the same buyer-need rules as the X and Reddit search tools.
Each new qualifying post costs 5 credits; the baseline, empty checks and posts already seen are free, and a post is never reported twice.
At most 10 new posts are reported per check, and the feed says how many more were left out.
A small monthly limit lowers that number further, and the watch pauses when the limit is used up.
An account can keep 5 topic watches, inside its 10 watches in all.
A reported change has the platform as its domain (`x.com` or `reddit.com`), the quoted need as its title and the post link as its source.
Treat a post as a reason to look at the thread, not as confirmed buying intent.
Webhooks and the web Signals page show topic changes like any other signal.

```sh
gtm watch create --name "Agency CRM buyers" --topic 'CRM for agencies' --source both --dry-run
gtm watch create --name "Agency CRM buyers" --topic 'CRM for agencies' --source both --max-credits N --yes
```

## Contact job changes

Use `--signal job-change` when the user wants to know that a person they already know, such as a past champion, moved company or title.
MCP and REST use `gtm_watch` with `signal: "job-change"` and the same quote and create flow; the people travel in `companies` as LinkedIn profile URLs or handles.
The CLI takes `--person URL` once per person, or `--file` with a `linkedin_url`, `profile` or `url` column.
Profiles are read once a month on our schedule, with no cadence option.
A person who changed company or title costs 40 credits per check, once however much changed; the baseline, empty checks and unreadable profiles are free.
A move is reported when the new company also shows in the person's headline, or when the same new position holds on two checks in a row.
Wording edits such as case, word order, abbreviations or a legal suffix are never a change, and a profile that cannot be read is skipped and never charged.
The limits are 200 people per watch and 500 targets per account, counted together with company domains.
A mandatory monthly limit also caps how many people one check reports, and the rest are reported on the next check.
Offer to find a changed person's new work email with the email finder as a next step; never run it without the user's budget.

```sh
gtm watch create --name "Champions" --file people.csv --signal job-change --dry-run
gtm watch create --name "Champions" --file people.csv --signal job-change --max-credits N --yes
gtm watch create --name "Champions" --person https://www.linkedin.com/in/ada --signal job-change --dry-run
```
