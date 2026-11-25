# SEO and AI visibility research

Select the tools that answer the user's question, then describe and quote each one before spending.
Use the maximum credit budget the user approved.
When the user wants several jobs, agree a total budget and keep each run within it.

## Full website workflow

Use the website and what it sells to choose relevant buyer searches and questions.
Quote every paid run with `dry_run` and track settled charges against one shared budget across the workflow.
If a quote exceeds the remaining budget, reduce the scope within the user's instructions or report which work you skipped.

1. **Keyword research.** Use `keyword-ideas` and `keyword-volume` to shortlist the ten most relevant keywords.
2. **Search results.** Run `google-search` for the top five and report the site's position where it appears in the returned results.
3. **Competitors.** Use `domain-seo-overview`, `domain-ranked-keywords` and `domain-ads` for the three most relevant competing domains in those results.
4. **AI visibility.** Use `ai-visibility-check` for five buyer questions across ChatGPT, Google AI Mode, Gemini and Perplexity.
5. **Site audit.** Run `seo-aeo-audit` for the website and include the report link when available.
6. **Backlinks.** Use `backlink-summary` to compare the website with its top search competitor.

Close with the three key findings and a 30-day action plan.

## Tool prices and inputs

| Capability | Estimated credits | What to send |
| --- | --- | --- |
| `keyword-ideas` | 2 to 5 credits | A seed keyword |
| `keyword-volume` | 14 to 27 credits | Up to 1,000 keywords |
| `keyword-trends` | 1 to 3 credits | Up to five keywords |
| `google-search` | 1 credit | A query and optional country and language codes |
| `google-news-search` | 1 credit | A query and optional country and language codes |
| `google-local-search` | 1 credit | A local query and optional country and language codes |
| `domain-seo-overview` | 1 to 2 credits | A bare domain such as `example.com` |
| `domain-ranked-keywords` | 2 to 4 credits | A bare domain |
| `domain-ads` | 4 to 6 credits | A bare domain |
| `ai-visibility-check` | 3 to 42 credits | A brand, its domain and up to ten buyer questions |
| `ai-answer` | 1 to 2 credits | One question and one engine |
| `seo-aeo-audit` | 39 credits | A website URL for a full report |
| `page-audit` | 1 credit | A page URL for on-page checks |
| `backlink-summary` | 4 to 6 credits | A bare domain |
| `backlink-list` | 1 to 4 credits | A bare domain |

AI visibility quotes 3 to 5 credits for one question across four engines and 24 to 42 credits for ten questions.
Keyword volume quotes from the submitted list and costs up to 14 credits for more than 50 distinct keywords.
Use the dry run's `quote` as the maximum for that input.
For cost-plus tools, the actual charge depends on the sources used, with the total rounded up once per run.
Empty results are free.

```sh
gtm tools describe keyword-volume
gtm tools run keyword-volume --input '{"keywords":["email finder","work email finder"]}' --dry-run
gtm tools run keyword-volume --input '{"keywords":["email finder","work email finder"]}' --max-credits N
gtm results <run_id> --csv > keyword-volume.csv
```

In MCP, discover with `gtm_tools`, then call the matching named tool, such as `gtm_keyword_volume`, with the same input fields plus `dry_run` or `max_credits`.
In REST, call `gtm_tools` with `action: "run"`, the capability ID, its `input` object and the budget.
Send `X-GTM-Answer-Version: 5` in REST to read `pricing.range`, `pricing.parts` and the settled `credit_breakdown`.
Use the saved run to retrieve full rows, answers and cited URLs.

Search volumes, traffic, paid keywords and backlinks are estimates.
A trends result measures relative interest on a 0 to 100 scale.
Recent ads are observations in a 30-day window.
For AI visibility, report coverage by question and engine and distinguish unavailable answers from answers that omitted the brand.

## Full SEO/AEO audit

```sh
gtm tools run seo-aeo-audit --input '{"url":"example.com"}' --dry-run
gtm tools run seo-aeo-audit --input '{"url":"example.com"}' --max-credits 39
```

A completed audit returns a report URL and can take several minutes: poll the saved run with `gtm results <run_id>` instead of starting another.
Failed audits use no credits; an uncertain supplier payment blocks another purchase until it is reconciled.
In MCP, quote with `gtm_seo_aeo_audit` and `{"url":"example.com","dry_run":true,"max_credits":0}`, then call it with `{"url":"example.com","max_credits":39}` under an agreed budget.
