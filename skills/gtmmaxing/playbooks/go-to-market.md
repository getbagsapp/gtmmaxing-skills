# Plan and start a go-to-market

The user has a product and wants customers.
The result is a short plan that ends in a first list of real people, each with a reason to talk now.

1. **Learn the buyer** with [brief](brief.md): look first, then one round of questions.

2. **Write the bet in four lines.**
   - Buyer: one job title at one kind of company (industry, size, place).
   - Pain: what that person already spends time or money on.
   - Signal: the public fact that shows the pain is live this month.
   - Offer: what you give in the first message that is useful without a reply.

3. **Choose the signal from the pain.**

   | The pain shows up as | Signal | Command |
   | --- | --- | --- |
   | A team being built | Hiring for the role | `gtm leads --hiring-role 'Founding SDR' --target N --dry-run` |
   | New money to spend | Recent funding | `gtm leads --funded --target N --dry-run` |
   | Using a tool you replace or connect to | Tech stack of named companies | `gtm tools run tech-stack --input '{"domain":"acme.com","technologies":["Salesforce"]}' --dry-run` |
   | Looking like your best customer | Lookalikes | `gtm leads --like acme.com --target N --dry-run` |
   | Saying so in public | X and Reddit posts | `gtm tools run reddit-search --input '{"query":"invoicing for freelancers","intent":"buyers","within":"month"}' --dry-run` |
   | Reacting to the topic | LinkedIn post engagers | `gtm leads --post https://www.linkedin.com/posts/example --target N --dry-run` |
   | A weak website or search presence | SEO/AEO audit | the local-website-audit or seo-research playbook |
   | Nothing public | Fit only | `gtm leads --titles 'Head of Finance' --industry SaaS --size 20-99 --target N --dry-run` |

   Fit comes first, then freshness, then strength: two independent signals on one account beat one.

4. **Get the list.**
   Quote the chosen signal at 30, 50 and 100 leads, ask how many the user wants, then run it with the find-leads playbook.
   Check the rows as in [brief](brief.md), replace the ones that miss the buyer line, and offer the next size when most fit.

5. **Save what worked**, free, as in [brief](brief.md).

6. **Hand over** the four-line bet, the list with one "why now" line for each person and its source, the first message built on the offer, and the single number that says whether to continue (replies or calls from this list).
   Outreach itself is the user's decision; EU and UK contacts need a lawful basis and an opt-out.

Done when the user has the bet, a first list or its quote waiting on their budget, and the number to watch.
