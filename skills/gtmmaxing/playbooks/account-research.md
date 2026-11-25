# Research an account

One company researched before outreach: what it does, what it uses, whether it is hiring, and who to contact.
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

3. Tech stack, when the user sells a tool that replaces or connects to the company's current ones:

   ```sh
   gtm tools run tech-stack --input '{"domain":"acme.com","technologies":["Salesforce","HubSpot"]}' --dry-run
   ```

   A tool that is not detected may still be in use, so treat a miss as unknown, not as a gap to pitch.

4. Decision-makers, narrowed to the titles the user sells to:

   ```sh
   gtm list --company acme.com --titles 'VP Sales' --titles 'Head of Revenue' --dry-run
   ```

5. Work emails for the people worth contacting, from the list run:

   ```sh
   gtm contacts --run-id <run_id> --target N --dry-run
   ```

6. Write the user a short account brief: what the company does and who it sells to, the tools it uses, recent hiring, the people to contact with their emails, and the credits spent.

The playbook is done when the brief covers every step the user wanted and names the run ids holding the full rows.
