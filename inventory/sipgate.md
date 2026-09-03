# sipgate GmbH inventory (discovery seed 2026-09-02)
# NOTE: hosts below are discovery candidates from passive DNS/CT; confirm in-scope vs program scope before active testing.
app.sipgate.de
dev.sipgate.de
login.sipgate.de
mail.sipgate.de
sipgate.de
www.sipgate.de

## PASSIVE RECON 2026-09-02 (read-only, non-intrusive)

> Recon observations only. These are NOT confirmed vulnerabilities; ownership/in-scope of each host must be confirmed against the program scope before any active testing. Hosts resolve + serve HTTP — investigation requires scoped authorization.

**Probed:** 6 hosts | **Live HTTP:** 2

| Host | Status | Server/Tech |
|---|---|---|
| `app.sipgate.de` | 301 | Server: nginx -> https://login.sipgate.com/ |
| `login.sipgate.de` | 301 | Server: nginx -> https://www.sipgate.de |

**CNAME review signals (3):**
- `mail.sipgate.de` -> `ghs.google.com`
- `app.sipgate.de` -> `web-redirects.service.sipgate.net`
- `login.sipgate.de` -> `web-redirects.service.sipgate.net`

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `app.sipgate.de` | **Ports:** [80, 443]
**Web surface only:** [80, 443]

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `login.sipgate.de` | **Ports:** [80, 443]
**Web surface only:** [80, 443]

## 2026-09-02 21:56:19 UTC

## 2026-09-02 23:58:18 UTC

## 2026-09-03 04:11:02 UTC

## 2026-09-03 09:00:28 UTC

## 2026-09-03 13:32:10 UTC

## 2026-09-03 17:20:36 UTC
- NEW `www.sipgate.de` — 200 OK, Cloudflare fronted marketing site (not in prior inventory as live)
- NEW `login.sipgate.com` — 302 to Keycloak OIDC auth realm `sipgate-apps`, client `sipgate-app-web`, implicit flow redirect to `app.sipgate.com`
- NEW `app.sipgate.com` — 200 OK, main SPA (Fastly/CDN), permissive CSP allowing `*.sipgate.com/*.de/*.co.uk/*.net`, WebSocket to `wss://*.sipgate.*`, Pusher, Intercom, Sentry
- NEW `sipgate.de` — 301 → `www.sipgate.de` (lighttpd)
- CHANGED `app.sipgate.de` — 301 → `login.sipgate.com` (was nginx redirect target, now confirmed live chain)
- CHANGED `login.sipgate.de` — 301 → `www.sipgate.de` (was nginx redirect target, now confirmed live chain)
- NEW `dev.sipgate.de` — no HTTP response (TCP timeout)
- NEW `mail.sipgate.de` — no HTTP response (CNAME → `ghs.google.com`, Google Workspace)
- NEW login.sipgate.com exposed as Apache+Keycloak realm `sipgate-apps`, OAuth2 IMPLICIT flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/ — the real cus
- NEW app.sipgate.com SPA: `/implicit-auth-redirect` reads client-controlled `redirect` from search, stores token, then `history.replace(redirect)` unvalidated (main.js `ImplicitAuthenticator`, main-C3206pW
- NEW OIDC discovery signals: grant `password`(ROPC), `client_secret_jwt`, id_token algs incl HS256/512, PKCE `plain`+`S256`.
- CHANGED dev.sipgate.de resolves to sipgate IP 217.10.68.23 but dead (no HTTP 80/443, timeout) — abandoned host, owned IP, no takeover.

## 2026-09-03 20:17:13 UTC

## 2026-09-03 22:33:27 UTC
