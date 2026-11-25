# Build a lead list

A new list of people or businesses, then contacts for the people on it.
When the user wants a number of leads with emails, use `gtm leads` instead (see the skill); this playbook is for reviewing a list before buying emails.

1. Pick the list type from the user's request:
   - People by job title, optionally in a country or city: `--titles` (up to 5) and `--location`.
     Give each job title as its own `--titles`, without commas; titles match job titles, not company types, so give the company type with `--industry`, `--size` and `--keywords`.
     One search returns up to 100 people; `--page 2` continues it.
   - Decision-makers at companies: `--company` with a domain, repeated for up to 100, optionally `--titles`.
     Each company costs 2 credits if the first source fills the search, or 18 if backup sources run and people are returned.
     Empty searches are free, and the dry run quotes the maximum across companies.
   - Local businesses: `--business-type` and `--location`; rows carry address, website and listed phone.

   Filters outside these are refused with the `fields:` the list accepts; tell the user which of their filters cannot be applied.

2. Quote it:

   ```sh
   gtm list --titles 'VP Sales' --location 'United States' --industry SaaS --size 20-99 --dry-run
   ```

3. Agree a budget with the user and run it:

   ```sh
   gtm list --titles 'VP Sales' --location 'United States' --industry SaaS --size 20-99 --max-credits 10
   ```

   Keep the `run_id:` from the answer.

4. Show the user the preview rows; page more by running the `next:` command each answer gives (never a fixed offset, pages can hold fewer than five rows), or save the list with `gtm results <run_id> --csv > list.csv`.

5. When the user wants emails for a people list, ask how many contacts they want as `N`, then quote and run contacts on the same run:

   ```sh
   gtm contacts --run-id <run_id> --target N --dry-run
   gtm contacts --run-id <run_id> --target N --max-credits B --out contacts.csv
   ```

   Here `B` is the budget the user agreed after seeing the dry-run quote for their number of people.
   Finding emails already includes the live mailbox check; add `--verify` only when the user asks for the optional extra recheck.
   A saved list holds at most 100 people per contacts run, so for a longer list fetch it page by page or export it with `gtm results <run_id> --csv`.

The playbook is done when the user has the list (and `contacts.csv` when they asked for emails) with the credits spent.
