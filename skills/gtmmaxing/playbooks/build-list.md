# Build a lead list

A new list of people or businesses, then contacts for the people on it.

1. Pick the list type from the user's request:
   - People by job title, optionally in a country or city: `--titles` (up to 5) and `--location`.
   - Decision-makers at one company: `--company` with a domain, optionally `--titles`.
   - Local businesses: `--business-type` and `--location`; rows carry address, website and listed phone.

   Filters outside these are refused with the `fields:` the list accepts; tell the user which of their filters cannot be applied.

2. Quote it:

   ```sh
   gtm list --titles 'VP Sales' --location 'United States' --limit 20 --dry-run
   ```

3. Agree a budget with the user and run it:

   ```sh
   gtm list --titles 'VP Sales' --location 'United States' --limit 20 --max-credits 10
   ```

   Keep the `run_id:` from the answer.

4. Show the user the preview rows; page more with `gtm results <run_id> --offset 5`, or save the list with `gtm results <run_id> --csv > list.csv`.

5. When the user wants emails for a people list, quote and run contacts on the same run:

   ```sh
   gtm contacts --run-id <run_id> --verify --dry-run
   gtm contacts --run-id <run_id> --verify --max-credits 60 --out contacts.csv
   ```

The playbook is done when the user has the list (and `contacts.csv` when they asked for emails) with the credits spent.
