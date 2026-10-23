# Refresh

The page is static. `index.html` renders everything from `data.json`; the ticker tape and GP chart are live TradingView embeds.

## Steps
1. Regenerate `data.json` with the same keys:
   - `asOf`, `status` (spx, spxChg, tnx, hy, updated)
   - `brief`: 5-6 items `{k, t, so}` ranked by M&A importance: short TAG, what happened with the number, "So what" for dealmakers (financing, valuations, timing, sectors, regulatory risk). Only use data that is elsewhere in this file. Add `k: "WATCH TODAY"` only for a known, dated catalyst.
   - `mk`: `{n, v, c, d, s, tv}` (tv = TradingView symbol or null)
   - `kpi`, `pulse`, `reg`, `regNote`
   - `prorata`: latest issue only; `items` are one-line takeaways + links. Never paste newsletter text (the repo is public).
   - `news.wsj` / `news.econ`: `{title, href}` only.
   No personal info, emails or account names. Don't fabricate; mark gaps "Unavailable".
2. Validate: `python3 -m json.tool data.json > /dev/null`
3. Publish:
   ```
   cd /workspace/dat-site
   git add data.json && git commit -m "Refresh data" && git push origin main
   ```
   GitHub Pages redeploys in about 1 minute.
