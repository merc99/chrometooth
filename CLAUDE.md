# Chrometooth: Claude Code handoff

Portfolio prototype by Ben Woods (benwoods.art, github.com/merc99). It's a single static page: `index.html` holds all HTML, CSS and JS, with images embedded. There's no build step.

## Ship it (first run)

1. `git init && git add . && git commit -m "Chrometooth prototype"`
2. Create the GitHub repo and push: `gh repo create merc99/chrometooth --public --source=. --push`
   (if `gh` isn't installed or authed, walk me through it or create the repo on github.com)
3. Deploy on Vercel: either `npx vercel --prod` (static, no framework, no build command) or import the repo at vercel.com/new.
4. Put the production URL into README.md (the "Live:" line), commit and push.

## Project notes

- The page is self-contained by design. Keep it a single file unless asked otherwise.
- `#app` in the URL opens the dashboard and `#app/activity` opens the Activity log. Everything else is the marketing site.
- Activity data comes from `buildRows()`, seeded with mulberry32, so it is identical on every load. Query syntax lives in `parseQuery()` / `compile()`; the view is the `AV` module.
- Theme: the user's light/dark choice is stored in localStorage key `ct-mode` and applied as `body.ct-dark` / `body.ct-light`.
- Demo data is fictional. IPs use 192.0.2.x, 198.51.100.x and 203.0.113.x, and phone numbers use 555-01xx.
- `assets/` holds the fang render and wordmark SVG for reuse (OG image, favicon, portfolio thumbnail).

## Optional: live AI on your own domain

Inside claude.ai the "Check something" panel uses `window.claude.use("sample")`. Off claude.ai that returns null and the panel shows Demo mode.

To make it live on Vercel:
1. Add `api/analyze.js` as a Vercel serverless function. It POSTs `{ text }`, calls the Anthropic Messages API with `ANTHROPIC_API_KEY` from env vars, sends the same prompt the page already builds (search for `You are Chrometooth, a network security analyst`), and returns the parsed JSON.
2. In `index.html`, where `sample` is resolved, fall back to `fetch('/api/analyze')` when `sample` is null, and treat it as live (update the "Live AI" pill).
3. Add basic protection: cap input at 6,000 chars, rate-limit per IP, and never expose the key to the client.
4. Set `ANTHROPIC_API_KEY` in Vercel project settings, redeploy and test with the four built-in examples plus a custom paste.

## Nice next steps

- Add a pixel-fang favicon (SVG) and an Open Graph image from `docs/hero.png`.
- Add a "Case study" link back to benwoods.art in the footer.
