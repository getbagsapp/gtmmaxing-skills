# Qualify a list of people

The user already has people (a CSV, a spreadsheet, earlier exports) and wants the best leads ranked.
Work from their file: spend credits on the facts it lacks, and grade by the criteria the user gave.

1. Read the file for free.
   Count the people, merge duplicates across files, and note which columns already exist: LinkedIn URL, title, company, email.
   Sort everyone into a first-pass order from the titles and companies in the file; this costs nothing and decides where paid checks go first.

2. Quote the paid checks the user's ask needs, for the people at the top of that order or for everyone:

   ```sh
   gtm tools run linkedin-profile --file people.csv --dry-run
   gtm tools run linkedin-activity --file people.csv --input '{"topics":["automation","robotics"]}' --dry-run
   gtm contacts --file missing-emails.csv --dry-run
   ```

   `linkedin-profile` confirms the current role and company.
   `linkedin-activity` gives the last post date, posts in 90 and 365 days, and posts on the user's topics, each with its date and link.
   The contacts command finds emails for the people the file has none for.
   Tell the user each quote and the total, and agree one budget.

3. Run the agreed checks with `--max-credits` and `--out`, each in one command per file.
   Each command batches the file itself and replays finished batches free when rerun.

4. Grade with the user's own criteria, using the checked facts: current role, activity dates and topic posts, and whether an email exists.
   Name the tiers, give each person's tier with the fact behind it, and keep people whose facts are still unknown in a separate group with what is missing.

5. Hand over one CSV of everyone with their tier, the counts per tier, and the credits spent.

The playbook is done when every person in the file has a tier or a stated reason they have none, and the user has the counts and the cost.
