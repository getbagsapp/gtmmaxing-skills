# Leads from a hiring signal

Companies hiring for a role, the decision-makers at each, and their work emails, in one CSV.
A company hiring its first Sales Development Representative, for example, is building an outbound team and has budget for sales tools.
When the user's request names a total budget, that is the agreed budget: run inside it without asking again.

1. Choose the roles and the company filters.
   Founding titles such as `Founding SDR` are the strongest sign of a first hire; a plain role also returns large companies.
   Recruiters and staffing firms are left out automatically.

2. Quote it:

   ```sh
   gtm leads --hiring-role 'Founding SDR' --hiring-role 'Founding Account Executive' --industry SaaS --size 5-249 --location 'United States' --target 25 --dry-run
   ```

3. Agree a budget with the user and run it:

   ```sh
   gtm leads --hiring-role 'Founding SDR' --hiring-role 'Founding Account Executive' --industry SaaS --size 5-249 --location 'United States' --target 25 --max-credits 100
   ```

   Without `--titles` it looks for founders and sales leaders; add `--titles` for other buyers.

4. Save the rows:

   ```sh
   gtm results <run_id> --csv > leads.csv
   ```

5. Tell the user how many leads they got against the target, the credits spent, and the `report:` line in plain words.

The playbook is done when `leads.csv` exists and the user has the counts and the cost.
