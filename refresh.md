# Refresh

The page is static. `index.html` renders everything from `data.json`; the ticker tape and GP chart are live TradingView embeds.

## Steps
1. Regenerate `data.json` with the same keys:
   - `asOf`, `status` (spx, spxChg, tnx, hy, updated)
   - `brief`: 5-6 items `{k, t, so}` ranked by M&A importance: short TAG, what happened with the number, "So what" for dealmakers (financing, valuations, timing, sectors). Only use data that is elsewhere in this file. Add `k: "WATCH TODAY"` only for a known, dated catalyst.
   - `mk`: `{n, v, c, d, s, tv, tvn}` (tv = TradingView symbol tested to render in the free widget, tvn = its label; FRED: symbols get daily bars with no range)
   - `kpi`, `pulse`
   - `prorata`: latest issue only; `items` are one-line takeaways + links. Never paste newsletter text (the repo is public).
   - `news.wsj` / `news.econ`: `{title, href}` only.
   - `x`: `{status:"ok"|"unavailable", asOf, note, news:[{t,src,time,url}], posts:[{t,src,time,url}]}` from the `x` MCP tools, ~3-4 calls: `search_news` (M&A/PE/deals, max_age_hours 36) + `search_news` (markets/Fed/macro, 24h) -> `news`; `search_posts_all` from:business/Reuters/FT/WSJmarkets/DealBook/axios with deal keywords, last 24h, recency -> `posts` (handle as src). One-line paraphrase, ET time, x.com link. 10-15 items each max. If calls fail set status "unavailable". Optionally check US trends (woeid 23424977); include only business-relevant ones.
   No personal info, emails or account names. Don't fabricate; mark gaps "Unavailable".
2. Validate: `python3 -m json.tool data.json > /dev/null`
3. Publish:
   ```
   cd /workspace/dat-site
   git add data.json && git commit -m "Refresh data" && git push origin main
   ```
   GitHub Pages redeploys in about 1 minute.
