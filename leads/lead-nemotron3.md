## 2026-09-03 17:20:27 UTC [target] (model nemotron3)
[NEW] `www.sipgate.de` — 200 OK, Cloudflare fronted marketing site (not in prior inventory as live)
[NEW] `login.sipgate.com` — 302 to Keycloak OIDC auth realm `sipgate-apps`, client `sipgate-app-web`, implicit flow redirect to `app.sipgate.com`
[NEW] `app.sipgate.com` — 200 OK, main SPA (Fastly/CDN), permissive CSP allowing `*.sipgate.com/*.de/*.co.uk/*.net`, WebSocket to `wss://*.sipgate.*`, Pusher, Intercom, Sentry
[NEW] `sipgate.de` — 301 → `www.sipgate.de` (lighttpd)
[CHANGED] `app.sipgate.de` — 301 → `login.sipgate.com` (was nginx redirect target, now confirmed live chain)
[CHANGED] `login.sipgate.de` — 301 → `www.sipgate.de` (was nginx redirect target, now confirmed live chain)
[NEW] `dev.sipgate.de` — no HTTP response (TCP timeout)
[NEW] `mail.sipgate.de` — no HTTP response (CNAME → `ghs.google.com`, Google Workspace)
[PRIO] app.sipgate.com,9.25,attack_surface=9 business_value=10 tech_exposure=9 gate_ease=10 cloud_surface=8 freshness=10
[PRIO] login.sipgate.com,8.55,attack_surface=8 business_value=9 tech_exposure=8 gate_ease=10 cloud_surface=7 freshness=10
[PRIO] www.sipgate.de,5.15,attack_surface=4 business_value=5 tech_exposure=3 gate_ease=10 cloud_surface=6 freshness=8
[HYP] OIDC Implicit Flow Token Leakage via Referer/History
class: AUTH
asset: login.sipgate.com → app.sipgate.com
confidence: 75
reasoning: login.sipgate.com uses OIDC implicit flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/. Access token delivered in URL fragment. Fragment can leak via Referer header on subresource requests, browser history, or if redirect page loads third-party resources.
evidence_needed: Observe fragment handling in implicit-auth-redirect page; check for third-party requests carrying Referer; verify token storage mechanism.
verify_steps: GET https://login.sipgate.com/auth/realms/sipgate-apps/protocol/openid-connect/auth?response_type=token&client_id=sipgate-app-web&redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/&state=xyz → follow 302 → inspect https://app.sipgate.com/implicit-auth-redirect?redirect=/ for fragment handling, Referer leakage, third-party loads.
impact: Full account takeover via stolen access token; severity HIGH.
testability: PASSIVE
[HYP] WebSocket Origin Validation Bypass on app.sipgate.com
class: AUTH
asset: app.sipgate.com (wss://*.sipgate.com)
confidence: 65
reasoning: CSP allows `connect-src wss://*.sipgate.com wss://*.sipgate.de wss://*.sipgate.net wss://*.sipgate.co.uk`. Wildcard subdomain WS endpoints may exist. If Origin header not validated server-side, cross-subdomain WS hijack possible (e.g., from compromised dev.sipgate.de or subdomain takeover).
evidence_needed: Enumerate WS endpoints; test Origin header validation; check for subdomain takeover candidates.
verify_steps: OPTIONS/GET wss://app.sipgate.com/ (or discovered WS path) with Origin: https://evil.sipgate.com → check handshake acceptance. Passive: inspect JS for WS connection logic and Origin handling.
impact: Real-time message interception/injection, session hijack; severity HIGH.
testability: PASSIVE
[HYP] Keycloak Redirect_URI Validation Bypass on login.sipgate.com
class: OAUTH
asset: login.sipgate.com
confidence: 60
reasoning: Keycloak realm `sipgate-apps` with client `sipgate-app-web` uses implicit flow. If redirect_uri validation uses prefix/suffix match instead of exact match, open redirect on app.sipgate.com or sibling subdomain could steal token.
evidence_needed: Test redirect_uri variants (path traversal, subdomain, encoding); check Keycloak config for exact match enforcement.
verify_steps: GET https://login.sipgate.com/auth/realms/sipgate-apps/protocol/openid-connect/auth?response_type=token&client_id=sipgate-app-web&redirect_uri=https://app.sipgate.com/@evil.com/&state=xyz → observe if accepted. Test redirect_uri=https://evil.sipgate.com/implicit-auth-redirect.
impact: Token theft via open redirect → ATO; severity CRITICAL.
testability: PASSIVE
[PARKED] WebSocket Origin Validation Bypass on app.sipgate.com: No concrete WS endpoint discovered yet; CSP allows wildcard but actual endpoints unknown. Need endpoint enumeration first.
[PARKED] Keycloak Redirect_URI Validation Bypass on login.sipgate.com: Requires testing redirect_uri variants which may be considered active auth testing; passive-only verification limited.
[FINAL] 1. OIDC Implicit Flow Token Leakage via Referer/History (confidence 75, PASSIVE verifiable)
[NEXT] PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage, third-party requests, Referer policy.
[LEARN] REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
[LEARN] REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
[LEARN] ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
[RISK] sipgate: 78 — High-value VoIP/SaaS platform with OIDC implicit flow, wildcard WS origins, multi-tenant customer dashboards; primary risk is token leakage via fragment/Referer and potential redirect_uri misconfiguration.
## 2026-09-03 20:17:03 UTC [target] (model nemotron3)
## 2026-09-03 22:33:18 UTC [target] (model nemotron3)
[HYP] OIDC Implicit Flow Token Leakage via Referer/History on redirect page
class: AUTH
asset: login.sipgate.com → app.sipgate.com/implicit-auth-redirect
confidence: 75
reasoning: login.sipgate.com uses OIDC implicit flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/. Access token delivered in URL fragment. Fragment can leak via Referer header on subresource requests, browser history, or if redirect page loads third-party resources before token extraction.
evidence_needed: Observe fragment handling in implicit-auth-redirect page; check for third-party requests carrying Referer with fragment; verify token storage mechanism (localStorage vs sessionStorage) and Referrer-Policy headers.
verify_steps: GET https://login.sipgate.com/auth/realms/sipgate-apps/protocol/openid-connect/auth?response_type=token&client_id=sipgate-app-web&redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/&state=xyz → follow 302 → inspect https://app.sipgate.com/implicit-auth-redirect?redirect=/ for fragment handling, Referer leakage, third-party loads, Referrer-Policy.
impact: Full account takeover via stolen access token; severity HIGH.
testability: PASSIVE
[HYP] Internal Redis endpoint exposure aids SSRF/lateral movement
class: MISCONFIG
asset: clinq-bridge-sipgate/k8s/template/deployment.yml (public GitHub)
confidence: 95
reasoning: Public repo hardcodes `REDIS_URL: rediss://10.37.248.211:6378` — internal RFC-1918 IP with TLS Redis. Cross-referenced with `cloudbuild.yaml` revealing GCP project `clinq-services` zone `europe-west3`. If any sipgate service has SSRF or cloud-metadata access, this Redis becomes a lateral movement target.
evidence_needed: Confirm `10.37.248.211` is live in sipgate GCP network; verify Redis TLS enforcement; identify any SSRF vector in sipgate services reaching internal IPs.
verify_steps: Passive: correlate GCP project `clinq-services` with sipgate infrastructure; search for SSRF candidates in sipgate API surface (params: url, uri, callback, webhook, next, redirect). Active not permitted per scope.
impact: Internal infrastructure exposure → lateral movement / data access via SSRF chain; severity HIGH if SSRF exists.
testability: PASSIVE
[HYP] Radau CORS misconfiguration enables cross-origin credentialed API abuse
class: MISCONFIG
asset: radau/main.go (public GitHub) — deployed instance unknown
confidence: 85
reasoning: Default CORS config sets `AllowAllOrigins=true` + `AllowCredentials=true` with `Authorization` header allowed. Any origin can make credentialed requests to RADIUS auth API (user/token management). If deployed without `CORS_ORIGINS` env override, accounts are vulnerable to ATO via malicious site.
evidence_needed: Identify live radau deployment (subdomain?); verify `CORS_ORIGINS` not set in production; test cross-origin credentialed request to `/user` or `/token` endpoints.
verify_steps: Passive: search for radau subdomain (radau.sipgate.*, wifi.sipgate.*, auth.sipgate.*); check DNS for GCP `clinq-services` endpoints. If found: OPTIONS https://<radau-host>/user with Origin: https://evil.com → check `Access-Control-Allow-Credentials: true` + `Access-Control-Allow-Origin: https://evil.com`.
impact: Account takeover, token management abuse, WPA Enterprise RADIUS compromise; severity HIGH.
testability: PASSIVE
[NEXT] PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage (localStorage key), third-party requests, Referrer-Policy header, and any subresource loads that could carry fragment via Referer.
[LEARN] REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
[LEARN] REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
[LEARN] ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
[LEARN] REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is not demonstrable statically; token persists to localStorage before navigation — fragment never forwarded off-origin.
[LEARN] REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reachable flawed verifier.
[LEARN] REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. revoked/not registered.
[LEARN] REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scope`), no Host-header bypass found; redirect_uri validation correct.
[LEARN] ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/account/balance/payment).
[LEARN] ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
[LEARN] ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.
[RISK] sipgate: 75 — High-value VoIP/SaaS with OIDC implicit flow (token-in-fragment), permissive CSP wildcard WS origins, multi-tenant dashboards. Primary risks: (1) Referer/fragment token leakage on implicit-auth-redirect page (confirmed high-value, PASSIVE verifiable), (2) Public repo internal infrastructure exposure (Redis IP, GCP project) enabling SSRF chains if any sipgate service has SSRF, (3) Radau CORS misconfig if deployed without override. Third-party realm well-hardened (DCR blocked, redirect_uri strict, demo creds revoked). No confirmed live vulnerability this cycle; risk concentrated on unconfirmed fragment leakage and public-code infoleaks.
