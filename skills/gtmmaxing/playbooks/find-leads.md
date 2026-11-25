# Find a number of leads with emails

`gtm leads` keeps searching until it has the target number of new leads with emails, or stops at the budget.
Each lead carries the reason it fits.

1. **Brief.** Who, where, how many and how much.
   When one is missing, use [brief](brief.md); a number in the request is the target, and otherwise ask how many and offer 30, 50 and 100, each quoted with `--dry-run`, and up to 500 when the user wants volume.
   A call without a target returns a free `target_required` quote; it never starts a search.

2. **Market as filters**, not as words in a title: `--industry`, `--size`, `--location` (where the person lives) and `--keywords` (words in the company's description).
   Add at most one signal: `--hiring-role`, `--funded`, `--like` or `--post`.
   A tool's name as the hiring role finds companies using it.
   Founding titles such as `Founding SDR` are the strongest sign of a first hire; a plain role also returns large companies, and staffing firms are left out.
   Without `--titles`, a hiring search looks for founders and sales leaders.
   Skip only companies the user asks to skip, with one `--exclude-domain acme.com` each; competitors stay in unless asked. The user's own work domain is left out by itself.
   The saved ICP fills any filter left out, and people found before are skipped.

3. **Quote:**

   ```sh
   gtm leads --hiring-role 'Founding SDR' --industry SaaS --size 5-249 --location 'United States' --exclude-domain customer.com --target N --dry-run
   ```

   It prints expected credits, a spending ceiling, the balance, the remaining agent cap and affordable sizes.
   When the target is more than the funds, say so and let the user choose a smaller target or add credits.
   Affordable sizes are estimates; `--max-credits` is the hard limit.

4. **Run** with the agreed budget:

   ```sh
   gtm leads --hiring-role 'Founding SDR' --industry SaaS --size 5-249 --location 'United States' --exclude-domain customer.com --target N --max-credits B
   ```

   A target above 100, up to 500, runs in the background as one list under one budget.
   `gtm leads` waits and prints `progress:` lines; with `--no-wait` it returns the run id.
   While `status: running`, run the `next:` command; if it still runs after that, tell the user and check later.

5. **Export**, free: `gtm results <run_id> --csv > leads.csv`.

6. **Report** leads against target, credits spent, and the `report:` line (what was rejected and why) in plain words.
   `stop: budget_reached` offers the rest for a larger budget: ask first.
   `stop: sources_exhausted` means the filters or the signal ran out: suggest wider ones.

Done when `leads.csv` exists and the user has the counts and the cost.
