# timelockedapp.com

Static site for TimeLocked: the home page answer engines read, plus the old
invite pages. Hand-edited; there is no generator any more (see below).

## What it serves

- `/` — the home page: what the app does, its limits, an FAQ, and JSON-LD
  (`SoftwareApplication` + `FAQPage`). The FAQ text is in the page twice, visible
  and in the JSON-LD: change both.
- `/llms.txt`, `/robots.txt`, `/sitemap.xml`, `/404.html` (without a 404 page,
  Pages answers every unknown path with the home page and a 200).
- `/p/<slug>/` — one page per old protocol, the target of a shared invite. The
  app dropped protocols and invite links in JOPA-699, so the link now just opens
  the app; the pages stay because old links and the AASA point here.
- `/.well-known/apple-app-site-association` — Universal Links. Apple fetches it
  over https, expects **200** and **application/json**, and **does not follow
  redirects**. `_headers` sets the content type, since the file has no extension.

## Deploy (Cloudflare Pages — the domain is already on Cloudflare)

1. Cloudflare dashboard → Workers & Pages → Create → Pages → connect this repo.
2. Framework preset **None**, build command empty, output directory `/`.
3. Custom domains → add `timelockedapp.com`. DNS records are added automatically.
4. Verify — must be `200` and `application/json`, with no redirect:

   ```bash
   curl -sI https://timelockedapp.com/.well-known/apple-app-site-association
   ```

5. In Xcode: app target → Signing & Capabilities → **+ Associated Domains** →
   `applinks:timelockedapp.com`.

If the apex ends up redirecting to `www`, either drop that redirect or declare
both hosts — Apple will not follow it.

## Editing

The pages used to be generated from the app's protocol catalog, which was
deleted from the app in JOPA-699, so edit the HTML directly.

Every claim must be one the App Store text makes, worded no stronger
(`TimeLocked/marketing/appstore/store-copy.md`, its rules and claim table).
