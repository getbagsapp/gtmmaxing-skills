# Leads from people asking for a tool

The user sells something and wants people who are asking for it right now: on X, on Reddit, or among Instagram brands and creators.
Posts are the signal; contacts come only from people whose identity the post or their profile makes clear.

1. Agree the phrases for free.
   Ask what the user sells and draft two to four phrases a buyer would write, such as `"looking for a CRM"`, `"alternative to HubSpot"` or `"recommend a cold email tool"`.
   X matches every bare word, so quote each phrase or join alternatives with `OR`.

2. Quote the searches, one per phrase and site:

   ```sh
   gtm tools run x-search --input '{"query":"\"looking for a CRM\"","within":"week"}' --dry-run
   gtm tools run reddit-search --input '{"query":"best CRM for a small team","within":"month"}' --dry-run
   ```

   Each nonempty search is one charge for up to 20 posts.
   Tell the user the quotes and the total, and agree one budget.

3. Run the agreed searches with `--max-credits`, then read every post yourself before going further.
   Drop vendors advertising their own product, agencies, giveaways and replies that only recommend something; many matching posts are sellers, not buyers.
   Keep posts where a person asks, compares or complains, and note what they asked for.

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
