# Leads from a hiring signal

Companies hiring for a role, the decision-makers at each, and their work emails, in one CSV.
A company hiring its first Sales Development Representative, for example, is building an outbound team and has budget for sales tools.
When the user's request names a total budget, that is the agreed budget for the whole plan: run every step inside it without asking again, and stop only when it runs out.

1. Find the companies hiring for the role:

   ```sh
   gtm signals 'Sales Development Representative' --location 'United States' --limit 20 --dry-run
   gtm signals 'Sales Development Representative' --location 'United States' --limit 20 --max-credits 1
   ```

   One search returns up to 20 companies.
   For more, search again with a nearby role or another location, and drop companies already found.

2. Find each company's website domain.
   Signal rows name the company but not its domain; use a domain you already know reliably, or search for it:

   ```sh
   gtm tools run google-search --input '{"query":"Acme official website"}' --max-credits 1
   ```

   Skip a company whose domain stays uncertain rather than guessing.

3. Find the decision-makers at each domain, one command per company:

   ```sh
   gtm list --company acme.com --titles 'VP Sales' --titles 'Head of Revenue' --limit 3 --max-credits 2
   ```

   Keep every `run_id:`.
   A company with no matching people costs nothing; note it and move on.

4. Collect the people into one file and find their emails:

   ```sh
   gtm results <run_id>... --csv > people.csv
   gtm contacts --file people.csv --dry-run
   gtm contacts --file people.csv --max-credits 100 --out leads.csv
   ```

   Set `--max-credits` to what is left of the budget.

5. Hand over `leads.csv` with, for each person, the company, the role it is hiring for and when it was posted, plus how many companies had no decision-maker or no domain, and the credits spent.

The playbook is done when `leads.csv` exists and the user has the counts and the cost.
