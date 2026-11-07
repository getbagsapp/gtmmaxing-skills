# Build a lead list

A new list of people or businesses, then contacts for the people on it.
When the user wants a number of leads with emails, use `gtm leads` instead (see the skill); this playbook is for reviewing a list before buying emails.

1. Pick the list type from the user's request:
   - People by job title, optionally in a country or city: `--titles` (up to 5) and `--location`.
     Give each job title as its own `--titles`, without commas; titles match job titles, not company types, so give the company type with `--industry`, `--size` and `--keywords`.
     One search returns up to 100 people; `--page 2` continues it.
   - Decision-makers at companies: `--company` with a domain, repeated for up to 100, optionally `--titles`.
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

4. Show the user the preview rows; page more with `gtm results <run_id> --offset 5`, or save the list with `gtm results <run_id> --csv > list.csv`.

5. When the user wants emails for a people list, quote and run contacts on the same run:

   ```sh
   gtm contacts --run-id <run_id> --verify --dry-run
   gtm contacts --run-id <run_id> --verify --max-credits 60 --out contacts.csv
   ```

The playbook is done when the user has the list (and `contacts.csv` when they asked for emails) with the credits spent.
