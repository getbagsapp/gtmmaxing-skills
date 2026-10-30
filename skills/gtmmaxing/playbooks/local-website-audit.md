# Local businesses with weak websites

Local businesses of one type, an SEO and AEO audit of each website, and the weakest ranked first with a way to reach the owner.
For web, SEO and marketing agencies, the audit is both the reason to reach out and something useful to send.
When the user's request names a total budget, that is the agreed budget for the whole plan: run every step inside it without asking again, and stop only when it runs out.

1. Find the businesses:

   ```sh
   gtm list --business-type Dentists --location 'Austin, TX' --limit 10 --dry-run
   gtm list --business-type Dentists --location 'Austin, TX' --limit 10 --max-credits 1
   ```

   Rows carry the address, the listed phone and the website.
   Drop businesses without a website; tell the user how many.

2. Audit each website, one command per business:

   ```sh
   gtm tools run seo-aeo-audit --input '{"url":"example.com"}' --dry-run
   gtm tools run seo-aeo-audit --input '{"url":"example.com"}' --max-credits 35
   ```

   Each completed audit costs 35 credits, so the budget decides how many sites to audit; audit in list order until it runs out.
   An audit can take several minutes: check each saved run with `gtm results <run_id>` instead of starting it again.
   A failed audit is free.

3. Read each report at its `reportUrl` and rank the businesses from weakest to strongest, with the two or three findings that matter most for each.

4. When the user wants the owner's email, find the owner at the business's domain and look up the email:

   ```sh
   gtm list --company example.com --titles 'Owner' --limit 1 --max-credits 2
   gtm contacts --run-id <run_id> --max-credits 1
   ```

   Many small businesses have no findable owner; keep the listed phone as their contact.

5. Hand over the ranked list: business, website, phone, owner email where found, report link and top findings, plus the credits spent.

The playbook is done when every audited business is ranked with its report link and the user has the cost.
