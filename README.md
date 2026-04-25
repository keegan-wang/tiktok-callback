# tiktok-callback

Static HTTPS redirector page for the AI Video Editor Smart app's TikTok OAuth
flow. TikTok requires an `https://` redirect URI — `http://localhost` is not
accepted — so this page forwards the `?code=&state=` query params to a local
loopback server the desktop app spins up during the auth flow.

## Deployment

Served via GitHub Pages at:

```
https://keegan-wang.github.io/tiktok-callback/
```

Register that exact URL in the TikTok Developer portal under your app's OAuth
redirect URIs.

## Flow

1. App opens `https://www.tiktok.com/v2/auth/authorize/...&state=<csrf>:<port>`
   in the user's system browser.
2. User approves; TikTok redirects to this page with `?code=<code>&state=<csrf>:<port>`.
3. This page's JS parses the `port` from `state` and `fetch`es
   `http://127.0.0.1:<port>/callback?code=...&state=...`.
4. The app's loopback server receives the code, exchanges it for an access
   token, and shuts down.

The page contains no secrets and stores nothing — it's a pure forwarder.
