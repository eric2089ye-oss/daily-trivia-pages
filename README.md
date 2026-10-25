# daily-trivia-pages

Static site generator pipeline for a daily trivia-answers website.

The public repository intentionally contains **no data and no site code** —
both are pulled at build time from a private Cloudflare KV namespace, and the
rendered pages are deployed to Cloudflare Pages.

## How a build works

1. `data/dives-all.json` (one entry per dive: prompts, official pool sizes,
   100-point answers) is downloaded from Cloudflare KV.
2. Build code is downloaded from KV and executed to rebuild the site.
3. `site/` output is deployed to Cloudflare Pages, pinged to IndexNow, and
   verified against the live site.

## Triggers

- `repository_dispatch` (`data-updated`) — primary path, fired by the
  upstream collector once fresh daily data lands in KV.
- `schedule` (05:35 UTC) — backstop.
- `workflow_dispatch` — manual runs.

## Repository layout

```
.github/workflows/daily-build.yml   # the entire pipeline
```

Everything else lives outside this repository (KV + the private source repo).
