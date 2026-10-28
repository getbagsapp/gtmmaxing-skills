# Research an account

One company researched before outreach: what it does, whether it is hiring, and who to contact.
Quote each paid step with `--dry-run` and agree the budget with the user before running it, as in the main skill.

1. Company profile and website sales brief:

   ```sh
   gtm research acme.com --dry-run
   gtm research acme.com --max-credits 5
   ```

2. Hiring signals, when the user cares about timing or growth:

   ```sh
   gtm signals 'Account Executive' --company acme.com --dry-run
   ```

3. Decision-makers, narrowed to the titles the user sells to:

   ```sh
   gtm list --company acme.com --titles 'VP Sales' --titles 'Head of Revenue' --dry-run
   ```

4. Work emails for the people worth contacting, from the list run:

   ```sh
   gtm contacts --run-id <run_id> --dry-run
   ```

5. Write the user a short account brief: what the company does and who it sells to, recent hiring, the people to contact with their emails, and the credits spent.

The playbook is done when the brief covers every step the user wanted and names the run ids holding the full rows.
