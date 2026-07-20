# timelockedapp.com

Static site behind TimeLocked invite links. Generated from the app's protocol
catalog (`TimeLocked/Domain/Entities/DecisionProtocol.swift`) so the copy here
can't drift from the copy in the app.

## What it serves

- `/` — landing, lists the protocols
- `/p/<slug>/` — one page per protocol, the target of a shared invite. Opens the
  app if it's installed, explains the rule if it isn't.
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

## Regenerating

The pages are generated from the Swift catalog; when protocol copy changes in
the app, regenerate rather than hand-editing here.
