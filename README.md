# ArtificialTranscendence.com Daily Briefing Worker

## Important
The Cloudflare screen labelled “Upload static files to update your Worker” only uploads static assets. It will NOT deploy a scheduled Worker from JavaScript. Use the Worker code editor or Wrangler.

## Included
- `src/index.js`: Worker source.
- `wrangler.toml`: daily cron and KV binding configuration.
- Daily schedule `30 1 * * *` = 07:00 India Standard Time.
- One current KV item: `current_briefing`; failed runs do not replace the prior edition.
- Categories: global finance, Indian share market, Technology & AI, Healthcare, Crypto Watch.

## Dashboard setup
1. Go back to Workers & Pages.
2. Create a Worker named `at-daily-briefing` using “Start with Hello World”.
3. Open its code editor and replace the starter code with all of `src/index.js`; Save and Deploy.
4. Create a KV namespace named `AT_DAILY_BRIEFING`.
5. In Worker Settings > Bindings, add KV binding variable `BRIEFING_KV` and select that namespace.
6. In Worker Settings > Variables and Secrets, add secret `OPENAI_API_KEY`. Do not put the key in chat, HTML, GitHub or the source file.
7. Add variable `OPENAI_MODEL` = `gpt-4.1-mini` if available to your API account.
8. In Worker Settings > Triggers, add Cron Trigger `30 1 * * *`.
9. Check Worker Logs after testing.

## Wrangler deployment alternative
Requires Node.js/npm and Cloudflare authentication. Replace the KV namespace ID in `wrangler.toml`, then from the package folder:
- `npm install -g wrangler`
- `wrangler login`
- `wrangler secret put OPENAI_API_KEY`
- `wrangler deploy`

## Test
Open `https://YOUR-WORKER.workers.dev/health` for health check and `/api/briefing` for current JSON. Before the first successful scheduled run, `/api/briefing` returns `503 not_published`.
ChatGPT subscription and OpenAI API billing are separate. Verify API billing/model access before production use. RSS feeds can change or be unavailable; inspect logs and briefing sources before connecting to the live homepage.
