# Learn the buyer

A short brief makes the list right the first time.
Look first, guess second, ask once, then check the rows before anyone calls them.

## Look first, free

Read the request and chat, `gtm workspace` (saved ICP and exclusions), the user's files, and their site with your own tools.
From the site take what they sell, customers they name, and where they sell.
Then name the three closest competitors, from one search of your own when the site does not say.

"Leads for X" means X is the seller; "leads at X" means people who work there.
Say which one you took in your first line.
Only when nothing says what the company sells, ask for its site or one line, and nothing else yet.

## Place them by evidence

| What you can see | What helps now |
| --- | --- |
| No customers yet | Find who answers. Pick the two buyer types most likely to feel the pain and offer leads for both, so replies show which one to build on. |
| Pilots or a few customers | Ask for the best two or three and why they bought. Find more like them by their shared traits or `--like`. |
| Many customers | The saved or stated ICP, plus one signal for timing. |

When you cannot see who pays already, ask it as one of the questions.
Every stage has a list that helps: use the answer to choose the list, and lead with what it gets them.

## Draft the brief

| Field | Where it goes | When nothing says |
| --- | --- | --- |
| Sells what, who already pays | `--notes` in the saved ICP | Ask |
| Buyer: the title that feels the pain | one `--titles` for each title | Ask, with three titles |
| Company: industry and size | `--industry`, `--size` | Ask, best guess first |
| Place: where the buyer lives | `--location` | Ask with no pick, unless their words, site or customers show it |
| Why now: one signal | `--hiring-role`, `--funded`, `--like` or `--post` | Recommend from the pain |
| Leave out: customers and the competitors you found | one `--exclude-domain` each on the search; the user's own work domain is left out by itself | Show your list in the same round |
| How many, for how much | `--target`, `--max-credits` | Always ask, with quoted sizes |

Buyers with a street address (dentists, restaurants, shops) are a local list: `gtm list --business-type Dentists --location Mumbai --dry-run`, far cheaper than `gtm leads`.
One local search returns up to 20 businesses, so quote 20 and offer more cities or areas for more.
`--keywords` describe the buyer's company: the seller's own category finds its competitors.
A competitor is also a signal: people complaining about it or hiring for it are the buyers.

## Ask once

Ask only what is still blank or weak, the one that changes the list most first, at most four questions in all.
Each is one line with two to four lettered options: your pick first, marked `(my pick)` with the fact behind it; with no fact, give options without a pick.
Make "How many leads?" the last question: dry-run 30, 50 and 100 first and print the credits beside each, so one reply also agrees the budget.
Offer sizes up to 500 the balance affords when the user wants volume.
A budget without a number gets the largest size whose quote fits it; a number in the request is the target, and is never reduced.
When you know what the seller sells, quote your draft in this same round; only when you do not, ask who and where now and quote after the reply.
Use the app's own question picker when this session has one; otherwise:

```text
I took this as customers for Payroll Co (payroll for restaurants; your site names 3 pubs).

1. Who already pays you?   a) The 3 pubs, name the best: ___  b) Nobody yet  c) Many, see: ___
2. Where are the buyers?   a) United Kingdom (my pick, prices in pounds)  b) elsewhere: ___
3. Why now?                a) Hiring staff (my pick, payroll grows)  b) Just funded  c) Just fit
4. How many leads?         a) 30, up to <quote> credits  b) 50, up to <quote>  c) 100, up to <quote>  d) other: ___

I will leave out payrollco.com, your 3 pubs, and 3 competitors: ___
Reply like `1b 2a 3a 4b`.
```

Every `<quote>` is a number a dry run printed in this session.
`go` takes your picks; the size is the user's choice, so ask for it when the reply has none.
When the reply changes the filters, quote again and run only inside the credits they accepted.
Before the paid run, match each answer to its flag; an answer without a flag is a wrong list.

## Ask nothing

Skip the questions when the request says who, where, how many and how much, names a file or profiles to enrich, or wants one fact.
Never ask for something already said, saved or on their site, and never ask a second round unless answers conflict.
When the user says to decide, decide, say your picks in one line, and quote.

## Other jobs

| Job | Ask only what is missing from |
| --- | --- |
| Local businesses | type, city, what you offer them |
| Hiring candidates | role, place, must-have skill, seniority |
| A watch | which accounts, which signal, monthly limit |
| Sales call, or people at one company | what you sell, whose job it is |
| Buyers in public posts | the problem in the buyer's words, where they talk |
| SEO and AI visibility | site, country, two competitors |

## Check the rows

Read every row, free with `gtm results <run_id> --csv`, and sort them into fits, unclear and does not fit.
Reject near neighbors: the seller's own people, competitors, agencies and consultants, and titles that only share a word with the buyer.
Report "6 of 10 fit", list the rejected ones with the reason, and call a blank verdict inconclusive.
Deliver every row; lead with the ones to contact and mark the rest, so the user decides.
Then fix the filter that let the wrong rows in and quote replacements, so the user gets the number of good leads they asked for.
When most fit, offer the next size with its quote.
Then ask one thing: "Which of these would you not call, and why?"

Offer to save the brief, free, so next time starts without questions:

```sh
gtm workspace set-icp --titles 'Owner' --industry Restaurants --size 10-49 --location 'United Kingdom' --notes 'We sell ...'
```

Done when the user has the rows that fit, the ones that do not and why, the cost, and the next step.
