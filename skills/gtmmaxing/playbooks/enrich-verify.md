# Enrich and verify a lead list

A CSV or list of LinkedIn profiles in, a CSV of work emails out, in one command.
`--verify` adds a mailbox check on each found email for extra credits; include it when the user wants verified emails, and leave it out when they asked to skip verification.
The CLI reads the profile column, drops duplicates, and runs the file in batches that share one `--max-credits` budget, so the file can be any length.

1. Quote the file:

   ```sh
   gtm contacts --file leads.csv --verify --dry-run
   ```

   Check the `profiles:`, `duplicates:` and `skipped:` lines against what the user expects.
   When the CLI cannot find the profile column, name it with `--column`.
   Add `--find email,phone` only when the user asked for mobiles too.

2. Tell the user the profile count, the `quote:` and the `balance:`, and agree a budget.

3. Run it with that budget and an output file:

   ```sh
   gtm contacts --file leads.csv --verify --max-credits 400 --out enriched.csv
   ```

   The command waits until every batch has finished.

4. Read the answer:
   - `status: completed`: the summary counts emails found and verdicts, and `enriched.csv` holds every row.
   - `reason: over_max_credits` (exit 3): the budget ran out; `unsent:` says how many people were not sent.
     Ask the user before rerunning with a larger budget; batches that already ran replay for free.
   - `status: running`: run the same command again to keep waiting; it replays finished batches and continues.
     If it still says `status: running` after that, tell the user the job continues on the server and rerun the command later; reruns never charge twice.
   - `status: error` (exit 1): a batch failed and `unsent:` says how many people were not sent.
     Rerunning the same command replays finished batches for free and retries the failed batch once under the same budget; if it fails again, tell the user.

5. Hand over `enriched.csv` (columns `name`, `title`, `email`, `verdict`, `url`, `status`) with the counts and the credits spent.
   Report `run_cost:` as the credits spent when the answer has it: it covers every batch, including ones charged before a rerun, while `cost:` counts only this command.
   In the `verdict` column, `deliverable` means the mailbox check accepted the address and `invalid` means it rejected it.

The playbook is done when `enriched.csv` exists and the user has the found count, the verdict counts and the cost.
