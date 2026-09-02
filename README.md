# Akshat Srivastava — Portfolio

Static portfolio site. Single self-contained `index.html`, no build step, no dependencies.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site — markup, design tokens, CSS, JS inline |
| `vercel.json` | CDN cache headers, security headers, clean URLs |
| `robots.txt` | Crawler directives + sitemap pointer |
| `sitemap.xml` | Single-URL sitemap for Search Console |

## Deploy to Vercel

### Option A — Git (recommended, gives you CI/CD)

1. Create a GitHub repo and push all four files to the **root**:
   ```bash
   git init
   git add .
   git commit -m "Portfolio site"
   git branch -M main
   git remote add origin git@github.com:<you>/portfolio.git
   git push -u origin main
   ```
2. Go to [vercel.com/new](https://vercel.com/new) → **Import Git Repository** → pick the repo.
3. Framework Preset: **Other**. Leave Build Command and Output Directory **empty** — it's static.
4. Click **Deploy**. Live at `<project>.vercel.app` in ~30 seconds.

Every `git push` to `main` now redeploys automatically. Pushes to other branches get preview URLs.

### Option B — CLI

```bash
npm i -g vercel
vercel          # preview deploy
vercel --prod   # production deploy
```

## Custom domain

1. Buy the domain (Cloudflare Registrar / Namecheap — `.dev` forces HTTPS, a nice touch).
2. Vercel → Project → **Settings → Domains** → add it.
3. Apply the DNS records Vercel shows:
   - Apex (`example.com`) → `A` record to `76.76.21.21`
   - `www` → `CNAME` to `cname.vercel-dns.com`
4. HTTPS provisions automatically via Let's Encrypt.

## After the domain is live — required

Replace the placeholder `https://akshatsrivastava.dev/` with your real URL in:

- `index.html` → `<link rel="canonical">`
- `index.html` → `og:url` and `og:image`
- `index.html` → JSON-LD `"url"` field
- `robots.txt` → `Sitemap:` line
- `sitemap.xml` → `<loc>`

## SEO checklist

- [ ] Verify the domain in [Google Search Console](https://search.google.com/search-console)
- [ ] Submit `sitemap.xml`
- [ ] Test the JSON-LD in the [Rich Results Test](https://search.google.com/test/rich-results)
- [ ] Generate a real `og.png` (1200×630) for social previews
- [ ] Run Lighthouse — target 95+ on Performance / SEO / Accessibility
- [ ] Optional: enable Vercel Analytics (Project → Analytics)

## Caching notes

`vercel.json` sets `s-maxage=86400, stale-while-revalidate=604800` on the HTML: Vercel's edge CDN
serves the cached copy for a day, then keeps serving it while revalidating in the background for a
week. Visitors always get an instant response; you still see updates promptly after a deploy, since
deploys purge the cache. Static assets get the standard immutable one-year policy.
