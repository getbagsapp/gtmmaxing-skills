# Leads from people asking for a tool

The user sells something and wants people who are asking for it right now: on X, on Reddit, or among Instagram brands and creators.
Posts are the signal; contacts come only from people whose identity the post or their profile makes clear.

1. Identify the topic and the kind of result the user wants.
   X and Reddit search default to `intent: "buyers"`: plain topics expand into explicit vocabulary aliases (X also adds buyer-language queries), then the server checks relevance and keeps requests, comparisons and first-person problems with quoted evidence.
   For example, `Looking for GTM solutions` searches related GTM language rather than requiring the literal word `solutions` in every post.
   Explicit X syntax stays literal: quote phrases, join alternatives with `OR`, and group alternatives before applying operators.
   Use `intent: "posts"` when the user wants all matching posts, such as competitor mentions or topic research, without buyer qualification.
   Reddit supports words, phrases, AND/OR/NOT, exclusions, subreddit:, author:, title: and selftext:; X-only operators are refused.
   Reddit checks full titles/bodies and dates, dedupes threads and follows supplier cursors for at most three pages; counts and exclusions remain in saved results even when nothing qualifies.
   The server refuses unsupported operators before spending.

2. Quote the searches, one per phrase and site:

   ```sh
   gtm tools run x-search --input '{"query":"Looking for GTM solutions","intent":"buyers","within":"week"}' --dry-run
   gtm tools run x-search --input '{"query":"\"looking for a CRM\"","intent":"posts","within":"week"}' --dry-run
   gtm tools run reddit-search --input '{"query":"CRM","intent":"buyers","within":"month"}' --dry-run
   ```

   X and Reddit buyer search cost one credit only when at least one post qualifies, for up to 20 delivered posts; bounded supplier calls are included in that search.
   Literal X and Reddit searches cost one credit when they return matching posts.
   Tell the user the quotes and the total, and agree one budget.

3. Run within the user's authorized credit budget and review each delivered post before going further.
   X and Reddit buyer rows include `signal`, `evidence` and `reason`; the summary reports qualified needs against unique candidates, rather than treating raw posts as leads.
   Qualification uses conservative English text rules and explicit vocabulary expansions, so nuanced requests and other languages can be missed.
   A stated need does not establish a budget, purchase timeline or permission for outreach.
   Read the exact evidence and linked post; discard any remaining ads, hiring posts, networking requests or resolved needs.
   An empty qualified result is free even if the supplier returned posts; widen the date window or adjust the topic rather than accepting irrelevant filler.

4. Turn kept X posts into people only when the identity is clear.
   When the post, the author's display name or their linked profile names their company, find the LinkedIn profile:

   ```sh
   gtm tools run profile-finder --input '{"name":"Ray Gibson","company":"Acme"}' --dry-run
   ```

   When nothing names the company, keep the post with its link for the user to reply to on X; never guess an employer from a name.

5. Use Reddit threads as research, not a contact list.
   Reddit users are usually anonymous: hand over the threads, the questions buyers ask and the subreddits where they ask, and never try to identify an anonymous author.

6. For Instagram brands or creators the user names, read each profile:

   ```sh
   gtm tools run instagram-profile --input '{"profile":"gymshark"}' --dry-run
   ```

   The email and phone are the contacts the account chose to show, usually a shared business inbox.

7. Find work emails for the LinkedIn profiles from step 4 with the contacts command, quoted first like every paid step.

The playbook is done when the user has the kept posts with their links and what each person asked for, the contacts found for people whose identity was clear, and the credits spent.
