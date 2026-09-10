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

## 2026-09-04 00:50:23 UTC

## 2026-09-04 05:09:31 UTC
- NEW login.sipgate.com exposed as Apache+Keycloak realm `sipgate-apps`, OAuth2 IMPLICIT flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/ — the real cus
- NEW app.sipgate.com SPA: `/implicit-auth-redirect` reads client-controlled `redirect` from search, stores token, then `history.replace(redirect)` unvalidated (main.js `ImplicitAuthenticator`, main-C3206pW
- NEW OIDC discovery signals: grant `password`(ROPC), `client_secret_jwt`, id_token algs incl HS256/512, PKCE `plain`+`S256`.
- CHANGED dev.sipgate.de resolves to sipgate IP 217.10.68.23 but dead (no HTTP 80/443, timeout) — abandoned host, owned IP, no takeover.

## 2026-09-04 09:52:41 UTC
- NEW api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across /contacts, /account, /numbers, /users, /authorization/userinfo — reflects any Origin + allows Credentials + exp
- CHANGED app.sipgate.com/implicit-auth-redirect: JS `ImplicitAuthenticator` confirmed reading `redirect` from search, persisting token to localStorage, then `history.replace(redirect)` unvalidated (both models
- CHANGED login.sipgate.com third-party realm: DCR blocked by Trusted Hosts (POST → 403), redirect_uri validation strict (invalid URI → 400), demo creds revoked (invalid_client) — well-hardened (KB REJECTED)
- NEW clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` (RFC1918) + GCP project `clinq-services` zone `europe-west3` (KB ACCEPTED MISCONFIG)
- NEW radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo (KB ACCEPTED MISCONFIG/SECRET)

## 2026-09-04 14:17:10 UTC
- NEW `chatbot.dev.sipgate.com` — live dev chatbot (nginx/1.24.0) with socket.io endpoint accessible from internet, loads React dev builds from unpkg.com
- NEW `chatbot.sipgate.com` — live production chatbot socket.io endpoint (`/chat/session/socket.io/`) accepts connections from any origin
- NEW `payment.sipgate.com` — live payment API (Java Spring, JSESSIONID), proper CORS (only reflects `app.sipgate.com`)
- NEW `team-de.live.sipgate.com` — live team portal (Apache/PHP), 302→login.sipgate.com; `frame-ancestors` whitelists `app.local.sipgate.com:3443` (local dev); `SERVERID=team-web03` leaked
- NEW `app.dev.sipgate.com` — live dev SPA on Fastly CDN, serves identical main-C5_XLhfX.js bundle as production (no IP restriction)
- NEW `api.dev.sipgate.com` — 403 Forbidden on all paths (WAF blocked, but accessible)
- NEW Production JS bundle hardcodes internal dev URLs: `api.local.sipgate.com:3396`, `app.local.sipgate.com:3443`, `payment.local.sipgate.com:8080`, `team-de.local.sipgate.com:10443`, `login.dev.sipgate.co
- NEW `api.sipgate.com/health` — unauthenticated endpoint (200 OK, "Healthcheck - OK") with full arbitrary-origin CORS + credentials reflected
- CHANGED `app.dev.sipgate.com` — CSP identical to production; JS references `api.dev.sipgate.com` in `pickByEnvironment` but dev API is 403-blocked
- CHANGED `chatbot.dev.sipgate.com` — `/chat/session/socket.io/?EIO=4&transport=polling` returns valid socket.io session from any origin (no CORS check)

## 2026-09-04 17:48:42 UTC
- NEW chatbot.dev.sipgate.com — live dev chatbot (nginx/1.24.0) with socket.io endpoint accessible from internet, loads React dev builds from unpkg.com
- NEW chatbot.sipgate.com — live production chatbot socket.io endpoint (/chat/session/socket.io/) accepts connections from any origin
- NEW payment.sipgate.com — live payment API (Java Spring, JSESSIONID), proper CORS (only reflects app.sipgate.com)
- NEW team-de.live.sipgate.com — live team portal (Apache/PHP), 302→login.sipgate.com; frame-ancestors whitelists app.local.sipgate.com:3443 (internal dev); leaks SERVERID=team-web03
- NEW app.dev.sipgate.com — live dev SPA on Fastly CDN, serves identical main-C5_XLhfX.js bundle as production (no IP restriction)
- NEW api.dev.sipgate.com — 403 Forbidden on all paths (WAF blocked, but accessible)
- NEW Production JS bundle hardcodes internal dev URLs: api.local.sipgate.com:3396, app.local.sipgate.com:3443, payment.local.sipgate.com:8080, team-de.local.sipgate.com:10443, login.dev.sipgate.com
- NEW api.sipgate.com/health — unauthenticated endpoint (200 OK, "Healthcheck - OK") with full arbitrary-origin CORS + credentials reflected
- CHANGED app.dev.sipgate.com — CSP identical to production; JS references api.dev.sipgate.com in pickByEnvironment but dev API is 403-blocked
- CHANGED chatbot.dev.sipgate.com — /chat/session/socket.io/?EIO=4&transport=polling returns valid socket.io session from any origin (no CORS check)

## 2026-09-04 20:05:47 UTC
- NEW chatbot.dev.sipgate.com — live dev chatbot (nginx/1.24.0) with socket.io endpoint accessible from internet, loads React dev builds from unpkg.com
- NEW chatbot.sipgate.com — live production chatbot socket.io endpoint (/chat/session/socket.io/) accepts connections from any origin
- NEW payment.sipgate.com — live payment API (Java Spring, JSESSIONID), proper CORS (only reflects app.sipgate.com)
- NEW team-de.live.sipgate.com — live team portal (Apache/PHP), 302→login.sipgate.com; frame-ancestors whitelists app.local.sipgate.com:3443 (internal dev); leaks SERVERID=team-web03
- NEW app.dev.sipgate.com — live dev SPA on Fastly CDN, serves identical main-C5_XLhfX.js bundle as production (no IP restriction)
- NEW api.dev.sipgate.com — 403 Forbidden on all paths (WAF blocked, but accessible)
- NEW Production JS bundle hardcodes internal dev URLs: api.local.sipgate.com:3396, app.local.sipgate.com:3443, payment.local.sipgate.com:8080, team-de.local.sipgate.com:10443, login.dev.sipgate.com
- NEW api.sipgate.com/health — unauthenticated endpoint (200 OK, "Healthcheck - OK") with full arbitrary-origin CORS + credentials reflected
- CHANGED app.dev.sipgate.com — CSP identical to production; JS references api.dev.sipgate.com in pickByEnvironment but dev API is 403-blocked
- CHANGED chatbot.dev.sipgate.com — /chat/session/socket.io/?EIO=4&transport=polling returns valid socket.io session from any origin (no CORS check)
- CHANGED api.sipgate.com/v2/* — arbitrary-origin CORS reflection with credentials confirmed across /contacts, /account, /numbers, /users, /authorization/userinfo (KB ACCEPTED MISCONFIG)

## 2026-09-04 22:21:10 UTC

## 2026-09-05 00:18:49 UTC
- NEW login.sipgate.com exposed as Apache+Keycloak realm `sipgate-apps`, OAuth2 IMPLICIT flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/ — the real cus
- NEW app.sipgate.com SPA: `/implicit-auth-redirect` reads client-controlled `redirect` from search, stores token, then `history.replace(redirect)` unvalidated (main.js `ImplicitAuthenticator`, main-C3206pW
- NEW OIDC discovery signals: grant `password`(ROPC), `client_secret_jwt`, id_token algs incl HS256/512, PKCE `plain`+`S256`.
- CHANGED dev.sipgate.de resolves to sipgate IP 217.10.68.23 but dead (no HTTP 80/443, timeout) — abandoned host, owned IP, no takeover.

## 2026-09-05 04:44:20 UTC
- NEW `login.sipgate.com` — Keycloak OIDC realm `sipgate-apps`, implicit flow, redirect to `app.sipgate.com/implicit-auth-redirect` (not in inventory)
- NEW `app.sipgate.com` — Main SPA on Fastly/CDN, permissive CSP, WebSocket to `wss://*.sipgate.*` (not in inventory)
- NEW `api.sipgate.com` — API v2 with arbitrary-origin CORS + credentials on `/contacts`, `/account`, `/numbers`, `/users`, `/authorization/userinfo`, `/health` (not in inventory)
- NEW `chatbot.sipgate.com` — Production socket.io at `/chat/session/socket.io/` accepts arbitrary-origin handshake (not in inventory)
- NEW `chatbot.dev.sipgate.com` — Dev chatbot (nginx/1.24.0) with socket.io accessible from internet (not in inventory)
- NEW `payment.sipgate.com` — Payment API (Java Spring, JSESSIONID), proper CORS (only `app.sipgate.com`) (not in inventory)
- NEW `team-de.live.sipgate.com` — Team portal (Apache/PHP), CSP `frame-ancestors` includes `app.local.sipgate.com:3443`, leaks `SERVERID=team-web03` (not in inventory)
- NEW `app.dev.sipgate.com` — Dev SPA on Fastly CDN, identical production JS bundle, hardcoded internal URLs (`api.local:3396`, `app.local:3443`, `payment.local:8080`, `team-de.local:10443`), no IP restrict
- NEW `api.dev.sipgate.com` — 403 Forbidden on all paths (WAF blocked but accessible) (not in inventory)
- NEW `sipgate-desktop-app.s3.eu-central-1.amazonaws.com` — Publicly listable S3 bucket with softphone installers 1.3.0–1.17.19 (not in inventory)
- CHANGED `app.sipgate.de` — Now 301 → `login.sipgate.com` (was nginx redirect target)
- CHANGED `login.sipgate.de` — Now 301 → `www.sipgate.de` (was nginx redirect target)
- CHANGED `dev.sipgate.de` — Resolves to 217.10.68.23 but dead (TCP timeout), no takeover
- CHANGED `api.sipgate.com/v2/*` — KB 2026-09-05: All documented high-value paths return 401 empty-body; authz uniformly enforced at edge; BOLA not reachable unauthenticated (requires AUTH_HELPED)
- CHANGED `chatbot.sipgate.com` — KB 2026-09-05: Polling transport serves `Vary: Origin` with no ACAO for arbitrary origin → cross-origin reads blocked; arbitrary-origin acceptance narrowed to WS transport only

## 2026-09-05 08:45:49 UTC
- NEW chatbot.sipgate.com WS transport: KB 2026-09-05 confirms polling transport blocks cross-origin reads (Vary:Origin, no ACAO); arbitrary-origin acceptance narrowed to WebSocket transport only (HUMAN_ONL
- NEW api.sipgate.com/v2 BOLA unauthenticated: KB 2026-09-05 REJECTED — all documented high-value paths return 401 empty-body; authz uniformly enforced at edge; cross-tenant BOLA requires AUTH_HELPED tenant
- NEW api.sipgate.com/v2/translations/{language}: KB 2026-09-05 REJECTED OTHER — arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-fallback); no LFI/traversal
- NEW api.sipgate.com 401 headers: KB 2026-09-05 ACCEPTED INFO — 401 responses leak x-b3-traceid (Zipkin) + vary:origin; descriptive only, OOS standalone
- NEW app.dev.sipgate.com bundle rotation: KB 2026-09-05 — dev bundle rotated to main-D04St2Sb.js (5.65MB); hardcodes new dev hosts admin.dev.sipgate.net (.net TLD), integration.dev.sipgate.com, payment.dev
- NEW payment.sipgate.com: KB 2026-09-05 ACCEPTED INFO — every path incl /actuator/health, /gateway/health → 307 to https://sipgate.io (Spring Gateway catch-all); actuator/MSLB-positive paths not exposed
- CHANGED sipgate-desktop-app.s3: KB 2026-09-05 re-confirmed ACCEPTED MISCONFIG — publicly listable S3 bucket with softphone installers 1.3.0–1.17.19 (stale since 2024-06-11); ACL/policy reads denied; write pat
- CHANGED login.sipgate.com third-party realm: KB 2026-09-05 REJECTED — re-read openid-configuration shows standard Keycloak defaults (DCR, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512, PKCE plain)

## 2026-09-05 12:12:08 UTC
- NEW chatbot.sipgate.com WS transport: KB 2026-09-05 confirms polling transport blocks cross-origin reads (Vary:Origin, no ACAO); arbitrary-origin acceptance narrowed to WebSocket transport only (HUMAN_ONL
- NEW api.sipgate.com/v2 BOLA unauthenticated: KB 2026-09-05 REJECTED — all documented high-value paths return 401 empty-body; authz uniformly enforced at edge; cross-tenant BOLA requires AUTH_HELPED tenant
- NEW api.sipgate.com/v2/translations/{language}: KB 2026-09-05 REJECTED OTHER — arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-fallback); no LFI/traversal
- NEW api.sipgate.com 401 headers: KB 2026-09-05 ACCEPTED INFO — 401 responses leak x-b3-traceid (Zipkin) + vary:origin; descriptive only, OOS standalone
- NEW app.dev.sipgate.com bundle rotation: KB 2026-09-05 — dev bundle rotated to main-D04St2Sb.js (5.65MB); hardcodes new dev hosts admin.dev.sipgate.net (.net TLD), integration.dev.sipgate.com, payment.dev
- NEW payment.sipgate.com: KB 2026-09-05 ACCEPTED INFO — every path incl /actuator/health, /gateway/health → 307 to https://sipgate.io (Spring Gateway catch-all); actuator/MSLB-positive paths not exposed
- CHANGED sipgate-desktop-app.s3: KB 2026-09-05 re-confirmed ACCEPTED MISCONFIG — publicly listable S3 bucket with softphone installers 1.3.0–1.17.19 (stale since 2024-06-11); ACL/policy reads denied; write pat
- CHANGED login.sipgate.com third-party realm: KB 2026-09-05 REJECTED — re-read openid-configuration shows standard Keycloak defaults (DCR, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512, PKCE plain)

## 2026-09-05 15:25:31 UTC

## 2026-09-05 17:42:02 UTC
- NEW chatbot.sipgate.com/chat/session/socket.io/ — WS transport arbitrary-origin acceptance confirmed (polling transport blocks cross-origin reads via Vary:Origin no ACAO)
- NEW app.dev.sipgate.com — dev bundle rotated to main-D04St2Sb.js (5.65MB); hardcodes new dev hosts admin.dev.sipgate.net (.net TLD), integration.dev.sipgate.com, payment.dev.sipgate.com, team-de/team-uk.d
- NEW api.sipgate.com/v2 — uniform edge authz confirmed: all documented high-value paths return 401 empty-body; 404 only for truly-unknown paths; no authz-drift/BOLA unauthenticated
- NEW api.sipgate.com/v2/translations/{language} — whitelist-with-fallback confirmed; arbitrary language values incl URL-encoded traversal return same 200 English dict; no LFI/traversal
- NEW api.sipgate.com — 401 responses leak x-b3-traceid (Zipkin) + vary:origin; descriptive header only
- NEW payment.sipgate.com — all paths incl /actuator/health, /gateway/health → 307 to https://sipgate.io (Spring Gateway catch-all); actuator/MSLB not exposed
- NEW sipgate-desktop-app.s3.eu-central-1.amazonaws.com — publicly listable S3 bucket re-confirmed; softphone installers 1.3.0–1.17.19 (stale since 2024-06-11); ACL/policy reads denied; write path NOT teste
- CHANGED login.sipgate.com third-party realm — openid-configuration re-read: standard Keycloak defaults (DCR, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512, PKCE plain); config-advertising not a vu
- CHANGED chatbot.sipgate.com — arbitrary-origin acceptance narrowed to WebSocket transport only (HUMAN_ONLY verification needed)
- CHANGED api.sipgate.com/v2 — BOLA unauthenticated REJECTED; cross-tenant BOLA requires AUTH_HELPED tenant pairs
- CHANGED app.dev.sipgate.com — dev env externally inert (login.dev dead, api.dev 403); weaker-auth/ATO path deflated to static info-leak only
- CHANGED team-de.live.sipgate.com — CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERID=team-web03 (persistent)
- CHANGED api.sipgate.com/health — unauthenticated arbitrary-origin CORS with credentials (persistent defense-in-depth gap)
- CHANGED api.sipgate.com/v2/* — arbitrary-origin CORS reflection with credentials across multiple endpoints (persistent)

## 2026-09-05 19:37:52 UTC

## 2026-09-05 21:50:48 UTC
- NEW team-uk.live.sipgate.com — live Apache/PHP UK team portal, second host in team-*.live family (302→login.sipgate.com /authenticate/?redirect= chain); CSP frame-ancestors app.local.sipgate.com:3443 + co
- CHANGED Prod CSP connect-src `*.sipgate.com:3396` (internal api.local port) family-wide (team-de AND team-uk); live probes api.sipgate.com:3396 + team-de.live:3396 → TCP timeout → no external listener.
- CHANGED login.sipgate.com/?redirect= — evil vs benign redirect → byte-identical 302 to Keycloak sipgate-apps authorize w/ hardcoded redirect_uri (app.sipgate.com/implicit-auth-redirect?redirect=/) → redirect 
- CHANGED sipgate-desktop-app.s3 ?versions — 439 distinct keys all VersionId=null (versioning disabled), IsTruncated=false; no deleted/hidden versions.
- CHANGED chatbot.sipgate.com/chat/session/socket.io/ — WS transport arbitrary-origin acceptance confirmed (polling transport blocks cross-origin reads via Vary:Origin no ACAO); KB 2026-09-05 REJECTED direct WS
- CHANGED sipgate-desktop-app.s3.eu-central-1.amazonaws.com — KB 2026-09-05 re-confirmed ACCEPTED MISCONFIG listable bucket; write path untested (HUMAN sign-off required per KB).
- CHANGED api.sipgate.com/v2 — KB 2026-09-05: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and /v1/* → uniform 401/404; userId-prefixe
- CHANGED app.dev.sipgate.com — KB 2026-09-05: dev bundle rotated to main-D04St2Sb.js (5.65MB); hardcodes new dev hosts admin.dev.sipgate.net (.net TLD), integration.dev.sipgate.com, payment.dev.sipgate.com, te
- CHANGED login.sipgate.com third-party realm — KB 2026-09-05: openid-configuration re-read shows standard Keycloak defaults (DCR, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512, PKCE plain); config-
- CHANGED payment.sipgate.com — KB 2026-09-05: every path incl /actuator/health, /gateway/health → 307 to https://sipgate.io (Spring Gateway catch-all); actuator/MSLB not exposed.
- NEW api.sipgate.com/v2/swagger.json — KB 2026-09-05: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec annotations, no server authz drift.
- CHANGED chatbot.sipgate.com/chat/session/socket.io/ — WS transport arbitrary-origin acceptance confirmed (polling transport blocks cross-origin reads via Vary:Origin no ACAO); KB 2026-09-05 REJECTED direct WS
- CHANGED sipgate-desktop-app.s3.eu-central-1.amazonaws.com — KB 2026-09-05 re-confirmed ACCEPTED MISCONFIG listable bucket; write path untested (HUMAN sign-off required per KB).
- CHANGED api.sipgate.com/v2 — KB 2026-09-05: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and /v1/* → uniform 401/404; userId-prefixe
- CHANGED app.dev.sipgate.com — KB 2026-09-05: dev bundle rotated to main-D04St2Sb.js (5.65MB); hardcodes new dev hosts admin.dev.sipgate.net (.net TLD), integration.dev.sipgate.com, payment.dev.sipgate.com, te
- CHANGED login.sipgate.com third-party realm — KB 2026-09-05: openid-configuration re-read shows standard Keycloak defaults (DCR, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512, PKCE plain); config-
- CHANGED payment.sipgate.com — KB 2026-09-05: every path incl /actuator/health, /gateway/health → 307 to https://sipgate.io (Spring Gateway catch-all); actuator/MSLB not exposed.
- NEW api.sipgate.com/v2/swagger.json — KB 2026-09-05: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec annotations, no server authz drift.
- CHANGED chatbot.sipgate.com/chat/session/socket.io/ — WS transport arbitrary-origin acceptance confirmed (polling transport blocks cross-origin reads via Vary:Origin no ACAO); KB 2026-09-05 REJECTED direct WS
- CHANGED sipgate-desktop-app.s3.eu-central-1.amazonaws.com — KB 2026-09-05 re-confirmed ACCEPTED MISCONFIG listable bucket; write path untested (HUMAN sign-off required per KB).
- CHANGED api.sipgate.com/v2 — KB 2026-09-05: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and /v1/* → uniform 401/404; userId-prefixe
- CHANGED app.dev.sipgate.com — KB 2026-09-05: dev bundle rotated to main-D04St2Sb.js (5.65MB); hardcodes new dev hosts admin.dev.sipgate.net (.net TLD), integration.dev.sipgate.com, payment.dev.sipgate.com, te
- CHANGED login.sipgate.com third-party realm — KB 2026-09-05: openid-configuration re-read shows standard Keycloak defaults (DCR, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512, PKCE plain); config-
- CHANGED payment.sipgate.com — KB 2026-09-05: every path incl /actuator/health, /gateway/health → 307 to https://sipgate.io (Spring Gateway catch-all); actuator/MSLB not exposed.
- NEW api.sipgate.com/v2/swagger.json — KB 2026-09-05: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec annotations, no server authz drift.

## 2026-09-05 23:45:04 UTC
- NEW `chatbot.dev.sipgate.com` — LIVE (HTTP/2 200, nginx/1.24.0, via Google Cloud), serves HTML with `x-robots-tag: noindex`, socket.io endpoint likely accessible — contrary to KB implication of dev env in
- NEW `team-uk.live.sipgate.com` — LIVE (from KB 2026-09-05), second team portal with identical CSP dev-origin leak (`frame-ancestors app.local.sipgate.com:3443`) + `connect-src *.sipgate.com:3396` — family
- CHANGED `app.dev.sipgate.com` — CSP reflects arbitrary Origin? No ACAO/ACAC headers returned on evil Origin probe; CSP unchanged (wildcard `*.sipgate.com:3396` connect-src, `wss://*.sipgate.*` WS).
- CHANGED `login.dev.sipgate.com` / `team-de.dev.sipgate.com` / `payment.dev.sipgate.com` — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout), confirming KB "externally inert".
- CHANGED `api.sipgate.com/v2/swagger.json` — Live spec (144 paths, global `security: []`), re-confirms stale annotations vs edge-401 enforcement.

## 2026-09-06 04:14:13 UTC
- NEW `chatbot.dev.sipgate.com` — LIVE (HTTP/2 200, nginx/1.24.0, Google Cloud), serves HTML with `x-robots-tag: noindex`, socket.io endpoint accessible — contradicts prior "dev env externally inert" assess
- NEW `team-uk.live.sipgate.com` — LIVE second team portal with identical CSP dev-origin leak (`frame-ancestors app.local.sipgate.com:3443` + `connect-src *.sipgate.com:3396`) — family-wide info disclosure 
- CHANGED `app.dev.sipgate.com` — CSP probe with evil Origin: no ACAO/ACAC headers returned; CSP unchanged (wildcard `*.sipgate.com:3396` connect-src, `wss://*.sipgate.*` WS)
- CHANGED `login.dev.sipgate.com` / `team-de.dev.sipgate.com` / `payment.dev.sipgate.com` — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout), confirming KB "externally inert"
- CHANGED `api.sipgate.com/v2/swagger.json` — Live spec (144 paths, global `security: []`), re-confirms stale annotations vs edge-401 enforcement
- CHANGED `chatbot.sipgate.com/chat/session/socket.io/` — WS transport REJECTED for arbitrary Origin (evil → 400 no-ACAO); polling transport also blocks cross-origin reads (Vary:Origin, no ACAO)

## 2026-09-06 08:40:48 UTC

## 2026-09-06 12:30:51 UTC

## 2026-09-06 15:53:17 UTC
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
- NEW 2026-09-06 REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to prod chatbot REJECT class
- NEW 2026-09-06 REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test → SecurityError on window.opener read; token fragment stays same-origin
- NEW 2026-09-06 ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally inert"
- NEW 2026-09-06 ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-src *.sipgate.com:3396)
- NEW 2026-09-06 ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401
- CHANGED api.sipgate.com/v2/*: cross-tenant BOLA confirmed requiring AUTH_HELPED tenant pairs (uniform edge auth, all high-value paths 401)
- CHANGED sipgate-desktop-app.s3: publicly listable bucket re-confirmed (439 keys, versioning disabled, write path untested — HUMAN sign-off required)

## 2026-09-06 17:45:31 UTC

## 2026-09-06 20:28:47 UTC

## 2026-09-06 22:14:00 UTC
- NEW integration.dev.sipgate.com — NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *`; hardcoded in production JS bundle from app.dev.sipgate.com
- NEW app.dev.sipgate.com — JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integration.dev.sipgate.com` — all resolve to sipgate-owned 217.1
- CHANGED chatbot.dev.sipgate.com WS — direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to prod chatbot REJECT class
- CHANGED api.sipgate.com/v2/doc/oauth2-redirect.html — Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment stays same-origin; unconditional opener callback inert c
- CHANGED team-uk.live.sipgate.com — confirmed second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-src *.sipgate.com:3396)
- CHANGED api.sipgate.com/v2/swagger.json — live spec (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthenticated

## 2026-09-07 00:12:56 UTC
- NEW `chatbot.dev.sipgate.com` — LIVE (HTTP/2 200, nginx/1.24.0, via Google Cloud), serves HTML with `x-robots-tag: noindex`, socket.io endpoint likely accessible — contrary to KB implication of dev env in
- NEW `team-uk.live.sipgate.com` — LIVE (from KB 2026-09-05), second team portal with identical CSP dev-origin leak (`frame-ancestors app.local.sipgate.com:3443`) + `connect-src *.sipgate.com:3396` — family
- CHANGED `app.dev.sipgate.com` — CSP reflects arbitrary Origin? No ACAO/ACAC headers returned on evil Origin probe; CSP unchanged (wildcard `*.sipgate.com:3396` connect-src, `wss://*.sipgate.*` WS).
- CHANGED `login.dev.sipgate.com` / `team-de.dev.sipgate.com` / `payment.dev.sipgate.com` — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout), confirming KB "externally inert".
- CHANGED `api.sipgate.com/v2/swagger.json` — Live spec (144 paths, global `security: []`), re-confirms stale annotations vs edge-401 enforcement.
- NEW `chatbot.dev.sipgate.com` — LIVE (HTTP/2 200, nginx/1.24.0, Google Cloud), serves HTML with `x-robots-tag: noindex`, socket.io endpoint accessible — contradicts prior "dev env externally inert" assess
- NEW `team-uk.live.sipgate.com` — LIVE second team portal with identical CSP dev-origin leak (`frame-ancestors app.local.sipgate.com:3443` + `connect-src *.sipgate.com:3396`) — family-wide info disclosure 
- CHANGED `app.dev.sipgate.com` — CSP probe with evil Origin: no ACAO/ACAC headers returned; CSP unchanged (wildcard `*.sipgate.com:3396` connect-src, `wss://*.sipgate.*` WS)
- CHANGED `login.dev.sipgate.com` / `team-de.dev.sipgate.com` / `payment.dev.sipgate.com` — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout), confirming KB "externally inert"
- CHANGED `api.sipgate.com/v2/swagger.json` — Live spec (144 paths, global `security: []`), re-confirms stale annotations vs edge-401 enforcement
- CHANGED `chatbot.sipgate.com/chat/session/socket.io/` — WS transport REJECTED for arbitrary Origin (evil → 400 no-ACAO); polling transport also blocks cross-origin reads (Vary:Origin, no ACAO)
- NEW integration.dev.sipgate.com — NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *`; hardcoded in production JS bundle from app.dev.sipgate.com
- NEW app.dev.sipgate.com — JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integration.dev.sipgate.com` — all resolve to sipgate-owned 217.1
- CHANGED chatbot.dev.sipgate.com WS — direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to prod chatbot REJECT class
- CHANGED api.sipgate.com/v2/doc/oauth2-redirect.html — Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment stays same-origin; unconditional opener callback inert c
- CHANGED team-uk.live.sipgate.com — confirmed second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-src *.sipgate.com:3396)
- CHANGED api.sipgate.com/v2/swagger.json — live spec (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthenticated
- NEW integration.dev.sipgate.com — NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *`; hardcoded in production JS bundle from app.dev.sipgate.com
- NEW app.dev.sipgate.com — JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integration.dev.sipgate.com` — all resolve to sipgate-owned 217.1
- CHANGED chatbot.dev.sipgate.com WS — direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to prod chatbot REJECT class
- CHANGED api.sipgate.com/v2/doc/oauth2-redirect.html — Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment stays same-origin; unconditional opener callback inert c
- CHANGED team-uk.live.sipgate.com — confirmed second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-src *.sipgate.com:3396)
- CHANGED api.sipgate.com/v2/swagger.json — live spec (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthenticated

## 2026-09-07 05:00:09 UTC
- NEW integration.dev.sipgate.com — NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *`; hardcoded in production JS bundle from app.dev.sipgate.com
- NEW app.dev.sipgate.com — JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integration.dev.sipgate.com` — all resolve to sipgate-owned 217.1
- CHANGED chatbot.dev.sipgate.com WS — direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to prod chatbot REJECT class
- CHANGED api.sipgate.com/v2/doc/oauth2-redirect.html — Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment stays same-origin; unconditional opener callback inert c
- CHANGED team-uk.live.sipgate.com — confirmed second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-src *.sipgate.com:3396)
- CHANGED api.sipgate.com/v2/swagger.json — live spec (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthenticated
- NEW integration.dev.sipgate.com — NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *`; hardcoded in production JS bundle from app.dev.sipgate.com
- NEW app.dev.sipgate.com — JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integration.dev.sipgate.com` — all resolve to sipgate-owned 217.1
- NEW integration.sipgate.com — PROD integration platform "Platypus" exposes full 26-op OpenAPI spec publicly under `/swagger` (contacts/call-logs/tasks/oauth2/streaming); spec auth = real login.sipgate.com
- NEW integration.dev.sipgate.com — dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same uniform 403 gate; extends prior "403 ACAO*" finding to docu
- NEW integration.sipgate.com — `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external requests return app-403 — spec-vs-behavior drift, gate must be
- CHANGED api.sipgate.com/v2/swagger.json — now returns 404 (was live 144-path spec per KB 2026-09-05/06)
- CHANGED chatbot.dev.sipgate.com — confirmed LIVE (nginx/1.24.0, Google Cloud) with socket.io endpoint; contradicts prior "dev env externally inert" assessment

## 2026-09-07 10:22:36 UTC
- NEW `api.sipgate.com/v2/doc/*` — live swagger-ui 5.x; implicit-only third-party client `sipgate-swagger-ui` exposes extreme scope set (oauth2-clients:write, balance:read, payment:methods:*, contacts/sms/a
- NEW `api.sipgate.com/v2/doc/keycloak-logout.js` — logout bridge always redirects to fixed same-origin oauth2-logout.html — not attacker-controllable, no open redirect (KB ACCEPTED INFO 2026-09-07)
- CHANGED `api.sipgate.com/v2/swagger.json` — now returns 404 (was live 144-path spec per KB 2026-09-05/06); spec relocated to /v2/doc/ swagger-ui or removed
- CHANGED `integration.sipgate.com` — full 26-op OpenAPI spec confirmed at /swagger with OAuth/SSRF chain potential; all external paths 403-gated; spec-vs-behavior drift on /oauth2/{redirect,callback} documente

## 2026-09-07 16:05:44 UTC
- NEW app.dev.sipgate.com — JS bundle rotated to `main-DFko0cRT.js` (from `main-Dr5Dd34d.js` 2026-09-06); Fastly CDN, 200 OK, identical production bundle
- NEW integration.sipgate.com — PROD "Platypus" integration platform exposes full 26-operation OpenAPI spec at `/swagger/swagger-ui-init.js` (contacts, call-logs, tasks, oauth2, streaming); all data paths r
- NEW integration.dev.sipgate.com — DEV twin serves near-identical 26-op spec (only auth host differs: `login.dev.sipgate.com`); HTTPS 403 with `access-control-allow-origin: *`; swagger bundle + sourcemap p
- NEW integration.sipgate.com — `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external requests return app-403 — spec-vs-behavior drift; `users.integ
- CHANGED api.sipgate.com/v2/swagger.json — now returns 404 (was live 144-path spec per KB 2026-09-05/06); spec relocated to `/v2/doc/` swagger-ui or removed
- CHANGED chatbot.dev.sipgate.com — confirmed LIVE (nginx/1.24.0, Google Cloud) with socket.io endpoint; contradicts prior "dev env externally inert" assessment (KB REJECTED 2026-09-05)
- CHANGED login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — externally inert confirmed

## 2026-09-07 19:58:37 UTC
- NEW app.dev.sipgate.com — JS bundle rotated to `main-DFko0cRT.js` (from `main-Dr5Dd34d.js` 2026-09-06); Fastly CDN, 200 OK, identical production bundle
- NEW integration.sipgate.com — PROD "Platypus" integration platform exposes full 26-operation OpenAPI spec at `/swagger/swagger-ui-init.js` (contacts, call-logs, tasks, oauth2, streaming); all data paths r
- NEW integration.dev.sipgate.com — DEV twin serves near-identical 26-op spec (only auth host differs: `login.dev.sipgate.com`); HTTPS 403 with `access-control-allow-origin: *`; swagger bundle + sourcemap p
- NEW integration.sipgate.com — `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external requests return app-403 — spec-vs-behavior drift; `users.integ
- CHANGED api.sipgate.com/v2/swagger.json — now returns 404 (was live 144-path spec per KB 2026-09-05/06); spec relocated to `/v2/doc/` swagger-ui or removed
- CHANGED chatbot.dev.sipgate.com — confirmed LIVE (nginx/1.24.0, Google Cloud) with socket.io endpoint; contradicts prior "dev env externally inert" assessment (KB REJECTED 2026-09-05)
- CHANGED login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — externally inert confirmed

## 2026-09-07 22:44:23 UTC
- NEW `integration.sipgate.com` — PROD "Platypus" integration platform exposes full 26-operation OpenAPI spec at `/swagger/swagger-ui-init.js` (contacts, call-logs, tasks, oauth2, streaming); all data paths
- NEW `integration.sipgate.com` — `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external requests return app-403 — spec-vs-behavior drift; `users.int
- NEW `integration.dev.sipgate.com` — DEV twin serves near-identical 26-op spec (only auth host differs: `login.dev.sipgate.com`); HTTPS 403 with `access-control-allow-origin: *`; swagger bundle + sourcemap
- CHANGED `app.dev.sipgate.com` — JS bundle rotated to `main-DFko0cRT.js` (from `main-Dr5Dd34d.js` 2026-09-06); Fastly CDN, 200 OK, identical production bundle
- CHANGED `api.sipgate.com/v2/swagger.json` — now returns 404 (was live 144-path spec per KB 2026-09-05/06); spec relocated to `/v2/doc/` swagger-ui or removed
- CHANGED `chatbot.dev.sipgate.com` — confirmed LIVE (nginx/1.24.0, Google Cloud) with socket.io endpoint; contradicts prior "dev env externally inert" assessment
- CHANGED `login.dev.sipgate.com` / `team-de.dev.sipgate.com` / `payment.dev.sipgate.com` — DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — externally inert confirmed

## 2026-09-08 01:23:37 UTC
- NEW login.sipgate.com exposed as Apache+Keycloak realm `sipgate-apps`, OAuth2 IMPLICIT flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/ — the real cus
- NEW app.sipgate.com SPA: `/implicit-auth-redirect` reads client-controlled `redirect` from search, stores token, then `history.replace(redirect)` unvalidated (main.js `ImplicitAuthenticator`, main-C3206pW
- NEW OIDC discovery signals: grant `password`(ROPC), `client_secret_jwt`, id_token algs incl HS256/512, PKCE `plain`+`S256`.
- CHANGED dev.sipgate.de resolves to sipgate IP 217.10.68.23 but dead (no HTTP 80/443, timeout) — abandoned host, owned IP, no takeover.
- NEW `*.integration.sipgate.cloud` (94 hosts via CT): per-vendor CRM adapters (hubspot, salesforce, zendesk, pipeforce...) all on GCP LB 35.246.154.68 behind nginx **Basic-auth 401** (distinct second gate 
- NEW `grafana.sipgate.cloud` + `grafana.aws.sipgate.cloud`: LIVE Grafana 11.5.1 internet-exposed (AWS 3.33.226.160), login-only, no anonymous read.
- NEW `share1.sipgate.cloud`: dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → **NXDOMAIN** — subdomain-takeover candidate.
- NEW CT `*.sipgate.cloud` enumeration (276 names): 146 `*.influxdb` monitoring hosts (Hetzner 168.119.232.113, externally inert), AWS+GCP multicloud wildcards (`*.eu-central-1.prod.aws`, `*.sandbox.dev.aws
- CHANGED `integration.sipgate.com/metrics` still public: `firebase_jwt_forbidden_requests 80373` (+574 vs prior KB), api_key 6, rate_limit 2500 → live Firebase-JWT validator confirmed.
- CHANGED swagger-ui-init.js: `.com`/`.cloud` **byte-identical** (386538b), no firebase/apiKey/AIza in bundle — token source not in spec bundle.
- CHANGED `app.dev.sipgate.com/assets/main-DFko0cRT.js` (5.65MB) has **zero** Firebase/AIza/identitytoolkit refs — token source not in public SPA.
- CHANGED `docs.sipgate.cloud` family → CNAME `sipgate.github.io` private GitHub Pages (302 GitHub auth) — sipgate-owned, NOT takeover.
- NEW `*.integration.sipgate.cloud` (94 hostnames via CT): per-vendor CRM adapter tier (hubspot, salesforce, zendesk, zapier, etc.) all on GCP LB 35.246.154.68; uniform nginx Basic-auth 401 (`WWW-Authentica
- NEW `grafana.sipgate.cloud` + `grafana.aws.sipgate.cloud` (CT): LIVE internet-facing Grafana 11.5.1, login-gated (no anonymous dashboards/API).
- NEW CT enumeration of `*.sipgate.cloud` (276 names): 146 `*.influxdb` internal monitoring hosts → 168.119.232.113 (externally inert); AWS/GCP/Hetzner multicloud + K8s tool cluster names (`nauticat.k8s-too
- CHANGED `integration.sipgate.com/metrics` still public: `firebase_jwt_forbidden_requests 80373` (+574), `api_key_forbidden_requests 6`, `rate_limit_forbidden_requests 2500` → live Firebase-JWT validator confi
- CHANGED swagger-ui-init.js byte-identical `.com`/`.cloud` (386538b), zero firebase/apiKey/AIza; `app.dev` main bundle (5.65MB) zero firebase refs → Firebase token source NOT in any public bundle.
- CHANGED `dev.integration.sipgate.cloud` / `test.integration.sipgate.cloud` → 404 inert behind same LB; `integration.dev.sipgate.com` still 403 ACAO* (TLS artifact earlier).
- CHANGED `docs.sipgate.cloud` family → private GitHub Pages (`sipgate.github.io`, 302 GitHub auth) — sipgate-owned org, NOT takeover.

## 2026-09-08 06:04:43 UTC
- NEW `*.integration.sipgate.cloud` (94 hosts via CT): per-vendor CRM adapters (hubspot, salesforce, zendesk, pipeforce...) all on GCP LB 35.246.154.68 behind nginx **Basic-auth 401** with `ACAO:*` CORS exp
- NEW `grafana.sipgate.cloud` + `grafana.aws.sipgate.cloud`: LIVE Grafana 11.5.1 internet-exposed (AWS 3.33.226.160), login-gated, no anonymous read
- NEW `share1.sipgate.cloud`: dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → **NXDOMAIN** — subdomain-takeover candidate
- NEW CT `*.sipgate.cloud` enumeration (276 names): 146 `*.influxdb` monitoring hosts (Hetzner 168.119.232.113, externally inert), AWS+GCP multicloud wildcards (`*.eu-central-1.prod.aws`, `*.sandbox.dev.aws
- CHANGED `integration.sipgate.com/metrics` still public: `firebase_jwt_forbidden_requests 80373` (+574 vs prior KB), `api_key_forbidden_requests 6`, `rate_limit_forbidden_requests 2500` → live Firebase-JWT val
- CHANGED `swagger-ui-init.js`: `.com`/`.cloud` **byte-identical** (386538b), no firebase/apiKey/AIza in bundle — token source not in spec bundle
- CHANGED `app.dev.sipgate.com/assets/main-DFko0cRT.js` (5.65MB) has **zero** Firebase/AIza/identitytoolkit refs — token source not in public SPA
- CHANGED `docs.sipgate.cloud` family → CNAME `sipgate.github.io` private GitHub Pages (302 GitHub auth) — sipgate-owned, NOT takeover
- CHANGED `dev.integration.sipgate.cloud` / `test.integration.sipgate.cloud` → 404 inert behind same LB; `integration.dev.sipgate.com` still 403 ACAO* (TLS artifact earlier)

## 2026-09-08 11:34:37 UTC
- NEW `*.integration.sipgate.cloud` (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with `ACAO:*` CORS, exposing `x-provider-url`/`x-provider-key` SSRF inputs (KB 2026-09-08)
- NEW `grafana.sipgate.cloud` + `grafana.aws.sipgate.cloud`: LIVE Grafana 11.5.1 on AWS LB (3.33.226.160), login-gated, no anonymous read (KB 2026-09-08)
- NEW `share1.sipgate.cloud`: dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN — subdomain-takeover candidate (KB 2026-09-08)
- NEW CT `*.sipgate.cloud` enumeration (276 names): 146 `*.influxdb` monitoring (Hetzner 168.119.232.113, inert), AWS/GCP multicloud wildcards, k8s tool clusters, dependency-track, docs (KB 2026-09-08)
- CHANGED `integration.sipgate.com/metrics`: `firebase_jwt_forbidden_requests 80373` (+574), `api_key_forbidden_requests 6`, `rate_limit_forbidden_requests 2500` — live Firebase-JWT validator confirmed (KB 2026
- CHANGED `swagger-ui-init.js`: `.com`/`.cloud` byte-identical (386538b), zero firebase/apiKey/AIza — token source not in spec bundle (KB 2026-09-08)
- CHANGED `app.dev.sipgate.com/assets/main-DFko0cRT.js` (5.65MB): zero Firebase/AIza/identitytoolkit refs — token source not in public SPA (KB 2026-09-08)
- CHANGED `docs.sipgate.cloud` family → CNAME `sipgate.github.io` private GitHub Pages (302 GitHub auth) — sipgate-owned, NOT takeover (KB 2026-09-08)
- CHANGED `dev.integration.sipgate.cloud` / `test.integration.sipgate.cloud` → 404 inert behind same LB; `integration.dev.sipgate.com` still 403 `ACAO:*` (KB 2026-09-08)

## 2026-09-08 15:21:20 UTC
- CHANGED *.integration.sipgate.cloud (94 hosts): per-vendor spec paths (/swagger/swagger-ui-init.js, /v3/api-docs, /swagger.json, /openapi.json) ALL 401 behind nginx Basic-auth — NO per-vendor spec reachable u
- CHANGED *.integration.sipgate.cloud CORS: OPTIONS preflight → 204 (passes nginx gate) with `ACAO:*` + `ACAC:true` + allow-headers `x-provider-locale,region,language,url,key,content-type`; data GET with `x-pro
- CHANGED vendor host breadth: hubspot/zendesk/salesforce.integration → nginx 401 (deployed); pipeforce/zapier.integration → resolve to same LB 35.246.154.68 but connection-reset (HTTP 000 ~0.3s) — per-hostname
- CHANGED integration.sipgate.com/oauth2/callback + /oauth2/redirect: both return app-403 `Forbidden resource` (application/problem+json) even with fabricated `code`/`state` params → callback does NOT mint Fire
- CHANGED integration.sipgate.com/metrics: `firebase_jwt_forbidden_requests 56363` LOWER than prior 80373 → per-replica counters (scale/topology info, restart-reset), not a stable global count.
- NEW `*.integration.sipgate.cloud` (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with `ACAO:*` CORS, exposing `x-provider-url`/`x-provider-key` SSRF inputs (KB 2026-09-08)
- NEW `grafana.sipgate.cloud` + `grafana.aws.sipgate.cloud`: LIVE Grafana 11.5.1 on AWS LB (3.33.226.160), login-gated, no anonymous read (KB 2026-09-08)
- NEW `share1.sipgate.cloud`: dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN — subdomain-takeover candidate (KB 2026-09-08)
- NEW CT `*.sipgate.cloud` enumeration (276 names): 146 `*.influxdb` monitoring (Hetzner 168.119.232.113, inert), AWS/GCP multicloud wildcards, k8s tool clusters, dependency-track, docs (KB 2026-09-08)
- CHANGED `integration.sipgate.com/metrics`: `firebase_jwt_forbidden_requests 80373` (+574), `api_key_forbidden_requests 6`, `rate_limit_forbidden_requests 2500` — live Firebase-JWT validator confirmed (KB 2026
- CHANGED `swagger-ui-init.js`: `.com`/`.cloud` byte-identical (386538b), zero firebase/apiKey/AIza — token source not in spec bundle (KB 2026-09-08)
- CHANGED `app.dev.sipgate.com/assets/main-DFko0cRT.js` (5.65MB): zero Firebase/AIza/identitytoolkit refs — token source not in public SPA (KB 2026-09-08)
- CHANGED `docs.sipgate.cloud` family → CNAME `sipgate.github.io` private GitHub Pages (302 GitHub auth) — sipgate-owned, NOT takeover (KB 2026-09-08)
- CHANGED `dev.integration.sipgate.cloud` / `test.integration.sipgate.cloud` → 404 inert behind same LB; `integration.dev.sipgate.com` still 403 `ACAO:*` (KB 2026-09-08)

## 2026-09-08 19:10:20 UTC
- CHANGED *.integration.sipgate.cloud (94 hosts): per-vendor spec paths (/swagger/swagger-ui-init.js, /v3/api-docs, /swagger.json, /openapi.json) ALL 401 behind nginx Basic-auth — NO per-vendor spec reachable u
- CHANGED *.integration.sipgate.cloud CORS: OPTIONS preflight → 204 (passes nginx gate) with `ACAO:*` + `ACAC:true` + allow-headers `x-provider-locale,region,language,url,key,content-type`; data GET with `x-pro
- CHANGED vendor host breadth: hubspot/zendesk/salesforce.integration → nginx 401 (deployed); pipeforce/zapier.integration → resolve to same LB 35.246.154.68 but connection-reset (HTTP 000 ~0.3s) — per-hostname
- CHANGED integration.sipgate.com/oauth2/callback + /oauth2/redirect: both return app-403 `Forbidden resource` (application/problem+json) even with fabricated `code`/`state` params → callback does NOT mint Fire
- CHANGED integration.sipgate.com/metrics: `firebase_jwt_forbidden_requests 56363` LOWER than prior 80373 → per-replica counters (scale/topology info, restart-reset), not a stable global count.

## 2026-09-08 21:54:26 UTC
- NEW `app.dev.sipgate.com` JS bundle rotated to `main-5xLTM2Hn.js` — new hardcoded hosts extracted: `admin.live.sipgate.net`, `api.local.sipgate.com:3396`, `app.local.sipgate.com:3443`, `payment.local.sipg
- NEW `share1.sipgate.cloud` dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN confirmed — subdomain-takeover candidate
- NEW `grafana.sipgate.cloud` live Grafana 11.5.1 confirmed via `/api/health` — login-gated, no anonymous
- CHANGED `integration.sipgate.com/metrics` `firebase_jwt_forbidden_requests` 69058 (+12,685 vs prior 56,363) — live Firebase-JWT validator confirmed, per-replica counter
- CHANGED `*.integration.sipgate.cloud` (hubspot.integration) — OPTIONS preflight 204 with `ACAO:*` + `ACAC:true` + `allow-headers: x-provider-url,x-provider-key`; data GET with `x-provider-url` header → 401 ng
- CHANGED `integration.sipgate.com/swagger/swagger-ui-init.js` — embedded spec confirms `/oauth2/redirect` + `/oauth2/callback` with NO security requirement; `users.integrations.create` accepts free-form `apiUr
- CHANGED `app.dev.sipgate.com` bundle contains 4 generic `apiKey` refs (analytics libs), zero Firebase/AIza/identitytoolkit — token source not in public SPA

## 2026-09-09 00:02:49 UTC
- NEW `app.dev.sipgate.com` JS bundle rotated to `main-5xLTM2Hn.js` — 13+ new hardcoded internal hosts extracted: `admin.live.sipgate.net`, `api.local.sipgate.com:3396`, `app.local.sipgate.com:3443`, `payme
- NEW `share1.sipgate.cloud` dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN confirmed — subdomain-takeover candidate
- NEW `grafana.sipgate.cloud` live Grafana 11.5.1 confirmed via `/api/health` — login-gated, no anonymous
- CHANGED `integration.sipgate.com/metrics` `firebase_jwt_forbidden_requests` 69058 (+12,685 vs prior 56,363) — live Firebase-JWT validator confirmed, per-replica counter
- CHANGED `*.integration.sipgate.cloud` (hubspot.integration) — OPTIONS preflight 204 with `ACAO:*` + `ACAC:true` + `allow-headers: x-provider-url,x-provider-key`; data GET with `x-provider-url` header → 401 ng
- CHANGED `integration.sipgate.com/swagger/swagger-ui-init.js` — embedded spec confirms `/oauth2/redirect` + `/oauth2/callback` with NO security requirement; `users.integrations.create` accepts free-form `apiUr
- CHANGED `app.dev.sipgate.com` bundle contains 4 generic `apiKey` refs (analytics libs), zero Firebase/AIza/identitytoolkit — token source not in public SPA

## 2026-09-09 04:34:03 UTC
- NEW `app.dev.sipgate.com` JS bundle rotated to `main-5xLTM2Hn.js` (2026-09-08) — 13+ hardcoded internal hosts extracted including `admin.live.sipgate.net`, `api.local.sipgate.com:3396`, `app.local.sipgate
- NEW `grafana.sipgate.cloud` live Grafana 11.5.1 confirmed via `/api/health` — login-gated, no anonymous
- CHANGED `integration.sipgate.com/metrics` `firebase_jwt_forbidden_requests` 71929 (+12,871 vs prior 59,058) — live Firebase-JWT validator confirmed, per-replica counter incrementing
- CHANGED `*.integration.sipgate.cloud` (hubspot.integration) — OPTIONS preflight 204 with `ACAO:*` + `ACAC:true` + `allow-headers: x-provider-url,x-provider-key`; data GET with `x-provider-url` header → 401 ng
- CHANGED `mock.integration.sipgate.cloud` — 404 on `/swagger/swagger-ui-init.js` but OPTIONS returns permissive CORS with `x-provider-*` headers allowed (distinct from gated siblings)

## 2026-09-09 09:31:09 UTC

## 2026-09-09 13:50:39 UTC
- NEW `mock.integration.sipgate.cloud` — ungated Express twin (only *.integration.sipgate.cloud host without nginx Basic-auth); `/health` 200, `/contacts` 200 (~8MB synthetic corpus), `/contacts/search`, `/
- CHANGED `integration.sipgate.com/metrics` — `firebase_jwt_forbidden_requests` 71929 (+12,871 vs prior 59,058); live Firebase-JWT validator confirmed, per-replica counter incrementing
- CHANGED `*.integration.sipgate.cloud` (hubspot.integration) — OPTIONS preflight 204 with `ACAO:*` + `ACAC:true` + `allow-headers: x-provider-url,x-provider-key`; data GET with `x-provider-url` header → 401 ng
- CHANGED `app.dev.sipgate.com` — JS bundle rotated to `main-5xLTM2Hn.js` (2026-09-08); 13+ hardcoded internal host:port pairs including `admin.live.sipgate.net` (prod subdomain), `api.local.sipgate.com:3396`, 
- CHANGED `grafana.sipgate.cloud` — live Grafana 11.5.1 confirmed via `/api/health` (version leak); login-gated, no anonymous access
- CHANGED `share1.sipgate.cloud` — dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN confirmed; subdomain-takeover candidate (needs HUMAN claim validation)

## 2026-09-09 17:48:05 UTC
- NEW `mock.integration.sipgate.cloud` — ungated Express twin (only *.integration.sipgate.cloud host without nginx Basic-auth); `/health` 200, `/contacts` 200 (~8MB synthetic corpus), `/contacts/search`, `/
- CHANGED `app.dev.sipgate.com` — JS bundle rotated to `main-CYk1JfU_.js` (2026-09-09 15:40:25 GMT); 18 hardcoded internal host:port pairs extracted including NEW `admin.live.sipgate.net` (prod subdomain), `api
- CHANGED `integration.sipgate.com/metrics` — `firebase_jwt_forbidden_requests` 70373 (per-replica counter, down from prior 71929 confirming restart-reset); live Firebase-JWT validator confirmed
- CHANGED `share1.sipgate.cloud` — dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN confirmed; subdomain-takeover candidate (needs HUMAN claim validation)
- CHANGED `grafana.sipgate.cloud` — live Grafana 11.5.1 confirmed via `/api/health` (version leak); login-gated, no anonymous access
- CHANGED `*.integration.sipgate.cloud` (94 hosts) — per-vendor CRM adapters uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} SSRF inputs; distinct second gate tier (nginx) v

## 2026-09-09 21:01:31 UTC
- NEW `admin.dev.sipgate.net` → `helpdesk.dev.sipgate.net` (217.116.120.148) — NEW prod-subdomain alias in dev JS bundle, HTTP 000 (timeout)
- NEW `admin.live.sipgate.net` → `helpdesk.live.sipgate.net` (217.10.73.71) — NEW prod subdomain in dev JS bundle, HTTP 000 (timeout)
- NEW `team-uk.dev.sipgate.com` (217.116.121.65) — NEW dev host in bundle, HTTP 000 (timeout)
- CHANGED `app.dev.sipgate.com` bundle rotated to `main-CYk1JfU_.js` (2026-09-09 15:40:25 GMT) — 18 hardcoded internal host:port pairs including 2 NEW prod subdomains
- CHANGED `integration.sipgate.com/metrics` `firebase_jwt_forbidden_requests` 2368 (down from 70373) — per-replica counter confirms restart-reset, live Firebase-JWT validator
- CHANGED `share1.sipgate.cloud` CNAME → `nx38603.your-storageshare.de` → NXDOMAIN confirmed — subdomain-takeover candidate
- CHANGED `grafana.sipgate.cloud` live Grafana 11.5.1 confirmed via `/api/health` — login-gated, no anonymous
- CHANGED `mock.integration.sipgate.cloud` ungated Express twin — `/contacts` 200 (~8MB synthetic), ACAO:* + ACAC:true + `allow-headers: x-provider-url,x-provider-key` — x-provider-url does NOT parameterize fet
- CHANGED `*.integration.sipgate.cloud` (94 hosts) — uniform nginx Basic-auth 401 gate + ACAO:* CORS preflight allowing `x-provider-url,x-provider-key` — data GET 401, header does not bypass nginx
- CHANGED `api.sipgate.com/v2/*` — arbitrary-origin CORS reflection with credentials persistent (evil.com reflected + ACAC:true + x-b3-traceid leak)
- CHANGED `team-uk.live.sipgate.com` CSP `frame-ancestors` includes `app.local.sipgate.com:3443` + `SERVERID=team-web02` — family-wide with team-de.live
- CHANGED `chatbot.dev.sipgate.com` live (nginx/1.24.0, GCP) — polling & WS transport both reject arbitrary Origin (400, Vary:Origin, no ACAO)

## 2026-09-09 23:16:23 UTC

## 2026-09-10 01:13:06 UTC
- NEW `admin.dev.sipgate.net` → `helpdesk.dev.sipgate.net` (217.116.120.148) — prod-subdomain alias in dev JS bundle, HTTP 000 (timeout)
- NEW `admin.live.sipgate.net` → `helpdesk.live.sipgate.net` (217.10.73.71) — prod subdomain in dev JS bundle, HTTP 000 (timeout)
- NEW `team-uk.dev.sipgate.com` (217.116.121.65) — dev host in bundle, HTTP 000 (timeout)
- CHANGED `app.dev.sipgate.com` bundle rotated to `main-CYk1JfU_.js` — 18 hardcoded internal host:port pairs including 2 NEW prod subdomains
- CHANGED `integration.sipgate.com/metrics` `firebase_jwt_forbidden_requests` 2368 (down from 70373) — per-replica counter confirms restart-reset, live Firebase-JWT validator
- CHANGED `share1.sipgate.cloud` CNAME → `nx38603.your-storageshare.de` → NXDOMAIN confirmed — subdomain-takeover candidate
- CHANGED `grafana.sipgate.cloud` live Grafana 11.5.1 confirmed via `/api/health` — login-gated, no anonymous
- CHANGED `mock.integration.sipgate.cloud` ungated Express twin — `/contacts` 200 (~8MB synthetic), ACAO:* + ACAC:true + `allow-headers: x-provider-url,x-provider-key` — x-provider-url does NOT parameterize fet
- CHANGED `*.integration.sipgate.cloud` (94 hosts) — uniform nginx Basic-auth 401 gate + ACAO:* CORS preflight allowing `x-provider-url,x-provider-key` — data GET 401, header does not bypass nginx
- CHANGED `api.sipgate.com/v2/*` — arbitrary-origin CORS reflection with credentials persistent (evil.com reflected + ACAC:true + x-b3-traceid leak)
- CHANGED `team-uk.live.sipgate.com` CSP `frame-ancestors` includes `app.local.sipgate.com:3443` + `SERVERID=team-web02` — family-wide with team-de.live
- CHANGED `chatbot.dev.sipgate.com` live (nginx/1.24.0, GCP) — polling & WS transport both reject arbitrary Origin (400, Vary:Origin, no ACAO)

## 2026-09-10 06:23:40 UTC

## 2026-09-10 11:51:37 UTC
- NEW app.dev.sipgate.com JS bundle rotated to `main-PqF61JxF.js` (5.65MB) — 18 hardcoded internal host:port pairs including NEW prod subdomains `admin.live.sipgate.net` (CNAME → helpdesk.live.sipgate.net, 
- NEW integration.sipgate.com/metrics: `firebase_jwt_forbidden_requests` 10839 (per-replica counter, restart-reset confirmed) — live Firebase JWT validator, auth-mechanism drift (spec Keycloak vs runtime Fi
- CHANGED *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters uniformly behind nginx Basic-auth 401 + ACAO:* CORS preflight allowing `x-provider-url,x-provider-key`; OPTIONS 204 passes gate, data GE
- CHANGED mock.integration.sipgate.cloud: ungated Express twin, `/contacts` 200 (~8MB synthetic), ACAO:* + ACAC:true + `allow-headers: x-provider-*` — empirical test confirms `x-provider-url` does NOT parameter
- CHANGED grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB (3.33.226.160), login-gated, version leaked via `/api/health`
- CHANGED share1.sipgate.cloud: dangling CNAME → `nx38603.your-storageshare.de` (Hetzner StorageShare) → NXDOMAIN — subdomain-takeover candidate (needs HUMAN claim validation)
- CHANGED team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP `frame-ancestors` includes `app.local.sipgate.com:3443` (internal dev origin) in production portals; `SERVERID` rotation (team-web02/team-web03
- CHANGED sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket (439 keys, 1.3.0–1.17.19, stale since 2024-06-11, versioning disabled); ACL/policy reads denied; write path NOT tested (H
- CHANGED api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints (evil.com reflected + ACAC:true + `x-b3-traceid` leak)
- CHANGED login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS256/HS384/HS512, PKCE plain, DCR gated by Trusted Hosts
- CHANGED chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, GCP) — both WS and polling transports reject arbitrary Origin (400, Vary:Origin, no ACAO); identical to prod REJECT class

## 2026-09-10 15:11:49 UTC
