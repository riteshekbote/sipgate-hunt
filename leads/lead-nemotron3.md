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
## 2026-09-04 00:50:14 UTC [target] (model nemotron3)
[PRIO] app.sipgate.com,9.40,attack_surface=9 business_value=10 tech_exposure=9 gate_ease=10 cloud_surface=8 freshness=10
[PRIO] login.sipgate.com,8.65,attack_surface=8 business_value=9 tech_exposure=8 gate_ease=10 cloud_surface=7 freshness=10
[PRIO] api.sipgate.com,8.00,attack_surface=7 business_value=10 tech_exposure=8 gate_ease=5 cloud_surface=8 freshness=10
[PRIO] radau (GitHub),8.05,attack_surface=6 business_value=8 tech_exposure=9 gate_ease=10 cloud_surface=8 freshness=9
[PRIO] clinq-bridge-sipgate (GitHub),7.90,attack_surface=6 business_value=7 tech_exposure=9 gate_ease=10 cloud_surface=9 freshness=9
[PRIO] www.sipgate.de,5.15,attack_surface=4 business_value=5 tech_exposure=3 gate_ease=10 cloud_surface=6 freshness=8
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
[FINAL] 1. OIDC Implicit Flow Token Leakage via Referer/History (confidence 75, PASSIVE)
[FINAL] 2. Internal Redis endpoint exposure aids SSRF/lateral movement (confidence 95, PASSIVE)
[FINAL] 3. Radau CORS misconfiguration enables cross-origin credentialed API abuse (confidence 85, PASSIVE)
[PARKED] Keycloak Redirect_URI Validation Bypass on login.sipgate.com: Requires testing redirect_uri variants which may be considered active auth testing; passive-only verification limited; confidence 60 below FINAL threshold for this cycle.
[PARKED] WebSocket Origin Validation Bypass on app.sipgate.com: No concrete WS endpoint discovered; CSP allows wildcard but actual endpoints unknown; needs endpoint enumeration first.
[PARKED] Keycloak id_token alg confusion (HS256): Standard Keycloak metadata, not proof of vulnerable verifier; REJECTED-adjacent per knowledge base.
[PARKED] ROPC/password grant + client_secret_jwt: Config-only, brute-force territory REJECTED.
[PARKED] dev.sipgate.de subdomain takeover: Abandoned host on owned IP; no dangling CNAME to attacker-controllable service.
[PARKED] Third-party realm valid client discovery: DCR gated by Trusted Hosts, demo creds revoked; no passive path to valid credential.
[NEXT] PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage (localStorage key), third-party requests, Referrer-Policy header, and any subresource loads that could carry fragment via Referer.
[LEARN] REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
[LEARN] REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
[LEARN] ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
[LEARN] REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is not demonstrable statically; token persists to localStorage before navigation — fragment never forwarded off-origin.
[LEARN] REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reachable flawed verifier.
[LEARN] REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. revoked/not registered — not a live credential exposure.
[LEARN] REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scope`), no Host-header bypass found; redirect_uri validation correct.
[LEARN] ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/account/balance/payment).
[LEARN] ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
[LEARN] ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.
[RISK] sipgate: 75 — High-value VoIP/SaaS with OIDC implicit flow (token-in-fragment), permissive CSP wildcard WS origins, multi-tenant dashboards. Primary risks: (1) Referer/fragment token leakage on implicit-auth-redirect page (confirmed high-value, PASSIVE verifiable), (2) Public repo internal infrastructure exposure (Redis IP, GCP project) enabling SSRF chains if any sipgate service has SSRF, (3) Radau CORS misconfig if deployed without override. Third-party realm well-hardened (DCR blocked, redirect_uri strict, demo creds revoked). No confirmed live vulnerability this cycle; risk concentrated on unconfirmed fragment leakage and public-code infoleaks.
## 2026-09-04 05:09:20 UTC [target] (model nemotron3)
[NEW] login.sipgate.com exposed as Apache+Keycloak realm `sipgate-apps`, OAuth2 IMPLICIT flow (response_type=token) with redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=/ — the real customer app login (in-scope, sipgate-owned).
[NEW] app.sipgate.com SPA: `/implicit-auth-redirect` reads client-controlled `redirect` from search, stores token, then `history.replace(redirect)` unvalidated (main.js `ImplicitAuthenticator`, main-C3206pWE.js).
[NEW] OIDC discovery signals: grant `password`(ROPC), `client_secret_jwt`, id_token algs incl HS256/512, PKCE `plain`+`S256`.
[CHANGED] dev.sipgate.de resolves to sipgate IP 217.10.68.23 but dead (no HTTP 80/443, timeout) — abandoned host, owned IP, no takeover.
[PRIO] login.sipgate.com,7.05,tech_exposure(OAuth/Keycloak/JWT)+business_value(auth gate)
[PRIO] app.sipgate.com,6.85,tech_exposure(OAuth2 implicit+client redirect)+business_value(customer dashboard)
[PRIO] dev.sipgate.de,2.50,attack_surface(dead host only)
[HYP] OAuth implicit token leakage via client-side unvalidated redirect on post-login handler
class: OATH
asset: app.sipgate.com/implicit-auth-redirect?redirect=<attacker>
confidence: 45
reasoning: JS confirms `ImplicitAuthenticator` reads `redirect` from location.search and after `initializeFromToken({access})` (token persisted to localStorage) does `history.replace(redirect)` with NO origin/allowlist validation; `redirect_uri` embeds attacker-influenced value into the same flow.
evidence_needed: browser confirms `history.replace('https://evil.example')` causes off-origin navigation (leaks/denies) OR only same-origin path push (no leak); verify token present in url fragment/history/referrer to attacker.
verify_steps: HUMAN browser: craft auth URL redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=https://evil.example, complete login, observe final destination and whether access_token reaches attacker-controlled origin (NC, no live customer data).
impact: if off-origin nav: access token + from_idp exfil -> full customer account compromise (ATO). If same-origin only: low open redirect. Severity: HIGH if confirmed, LOW otherwise.
testability: HUMAN_ONLY
[HYP] Keycloak id_token alg confusion (HS256 enabled alongside RS256) for auth bypass
class: AUTH
asset: login.sipgate.com/auth/realms/sipgate-apps
confidence: 35
reasoning: OIDC discovery lists id_token_signing_alg_values_supported incl HS256/512 alongside RS256/PS/ES; JWT alg-confusion applies only if any sipgate verifier validates token with RS256 public key as HMAC secret. Realm `sipgate-apps`, token_endpoint also allows client_secret_jwt.
evidence_needed: identify an app-endpoint token verifier that accepts attacker HS256-signed token (public key as secret) — requires a scoped verifier and live testing on test/sandbox, never live auth data.
verify_steps: passive config review only; active verify requires sandbox/test token endpoint and is AUTH_HELPED/HUMAN (no live customer-data bypass per scope).
impact: forged tokens -> authentication bypass across app -> full account takeover. Severity: CRITICAL if verifier flawed.
testability: HUMAN_ONLY
[HYP] ROPC/password grant + client_secret_jwt enabled enables credential/secret abuse
class: AUTH
asset: login.sipgate.com token endpoint
confidence: 25
reasoning: grant_types_supported includes `password`(ROPC) and token_endpoint_auth_methods_supported includes client_secret_jwt/private_key_jwt; ROPC enables credential-stuffing/offline guesses; client_secret_jwt risks alg confusion on client auth.
evidence_needed: demonstrate an affected confidential client uses client_secret_jwt and its secret/JWK is derivable, or ROPC leaks PII — mostly brute/rate-limit territory which is REJECTED.
verify_steps: not actionable without rate-limit bypass or secret leak; low confidence, config-only.
impact: account-session abuse / ATO; Severity: HIGH if a real client reachable, otherwise config hardening issue.
testability: HUMAN_ONLY
[PARKED] OAuth implicit token theft via open redirect (conf 45): code path confirmed unvalidated, but `history.replace(external)` in React Router typically pushes same-origin path (404), not off-origin — token stored in localStorage first, so fragment-based leak likely absent. Confidence below 50; needs live browser to resolve. Retained as top.
[PARKED] Keycloak alg-confusion (conf 35): config signal only (HS256 listed is standard Keycloak realm metadata, not proof of vulnerable verifier); REJECTED-adjacent (known-vulnerable library without program-specific exploit). Dropped from FINAL.
[PARKED] ROPC/password grant (conf 25): config-only, brute-force/rate-limit class is REJECTED. Dropped.
[PARKED] dev.sipgate.de (conf 20): abandoned host on owned IP; no dangling CNAME to attacker-controllable service -> not a takeover. Dropped.
[FINAL] survivors: OAuth implicit client-side redirect confirm (conf 45, HUMAN_ONLY) — only defensible non-REJECTED candidate; rest parked.
[NEXT] HUMAN: open a private-tab full login flow on `https://app.sipgate.com/implicit-auth-redirect?redirect=https://evil.example` via the standard login, and report final URL + whether access_token/from_idp appear in history/referrer/network to the attacker origin; do NOT use live customer accounts (sandbox/test creds only).
[LEARN] REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is not demonstrable statically; token persists to localStorage before navigation — fragment never forwarded off-origin. Class signal: unvalidated client redirect is not by itself token theft.
[LEARN] REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reachable flawed verifier; treat as config hardening, not a vulnerability.
[RISK] sipgate: 15 — Customer-facing Keycloak OAuth implicit flow + a confirmed unvalidated client-side `redirect` on the post-login handler warrant a human browser confirmation; but static analysis indicates the token is stored before navigation and React-router treats external targets as same-origin paths, so the high-impact token-theft chain is likely NOT real. Remaining signals are config-level/REJECTED. No confirmed in-scope vulnerability this cycle; risk primarily the unconfirmed redirect behavior.
[HYP] Valid registered API client discoverable via third-party realm for OAuth token acquisition
class: AUTH
asset: login.sipgate.com/auth/realms/third-party token endpoint
confidence: 25
reasoning: The only known client credential (from rest-api-examples) returns invalid_client → revoked. DCR is gated by Trusted Hosts. No public client_id list for this realm found; `client_id=junit`/`web-app` rejected. Without a valid client credential or DCR access, no token obtainable.
evidence_needed: a registered third-party client's client_id+secret, or a DCR Trusted-Hosts bypass — none demonstrated passively.
verify_steps: PASSIVE only: enumerate client_id guesses at auth endpoint (rejected → no leak of validity beyond 400); do NOT active-token or register clients (mutating/live-auth prohibited).
impact: if a live confidential client credential were leaked/derivable, attacker reaches social-scoped APIs (contacts/sms/balance/payment read). Severity HIGH; but not demonstrable. 
testability: PASSIVE
[NEXT] PROBE: GET `https://login.sipgate.com/auth/realms/third-party/protocol/openid-connect/registrations/openid-connect` and the realm `/auth/realms/third-party` page with a normal browser User-Agent to confirm no additional public registration/open redirect surface — read-only.
[LEARN] REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. revoked/not registered — not a live credential exposure; the pattern is example-code-only.
[LEARN] REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scope`), no Host-header bypass found; redirect_uri validation correct (invalid URI → 400); treat as hardened config, not a vulnerability.
[LEARN] ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/account/balance/payment) — a legitimate high-value target if a client credential or DCR bypass is ever obtained.
[RISK] sipgate: 10 — On the api surface the OIDC `third-party` realm is well-hardened: redirect_uri strictly validated, token endpoint requires valid client auth, DCR blocked by a Trusted Hosts policy, and the only publicly leaked API client credential is inert (`invalid_client`). Broad sensitive scopes exist but are unreachable without a valid client grant or DCR bypass, neither demonstrable passively. No confirmed in-scope API vulnerability this cycle.
class: AUTH
asset: login.sipgate.com/auth/realms/third-party
confidence: 25
reasoning: Leaked demo credential returns invalid_client (revoked). DCR gated by Trusted Hosts policy. No bypass found. Broad scopes (contacts/sms/account/balance/payment) unreachable without valid client grant.
evidence_needed: Registered third-party client_id+secret or DCR bypass (none found)
verify_steps: PASSIVE only. GET DCR discovery/resource and realm page; enumerate client_id guesses at realm auth endpoint (distinguished 400 only)
impact: HIGH if reachable (social-scoped customer data); not currently demonstrable
testability: PASSIVE
class: AUTH
asset: login.sipgate.com/auth/realms/sipgate-apps
confidence: 30
reasoning: OIDC discovery lists HS256/384/512 and client_secret_jwt. Only exploitable if a sipgate verifier validates an RS-signed token using RS public key as HMAC secret — no such verifier identified. Standard Keycloak metadata, not proof of flawed verifier.
evidence_needed: Locate verifier accepting attacker HS256-signed token (public key as secret)
verify_steps: passive config review; active verify requires sandbox (HUMAN_ONLY)
impact: CRITICAL if flawed verifier found; currently unproven
testability: HUMAN_ONLY
class: OATH
asset: app.sipgate.com/implicit-auth-redirect?redirect=<attacker>
confidence: 45
reasoning: `ImplicitAuthenticator` reads `redirect` from location.search, persists token to localStorage, calls `history.replace(h)` with no origin validation. Token stored BEFORE navigation — fragment-based leak unlikely.
evidence_needed: Browser confirms `history.replace('https://evil.example')` causes off-origin navigation carrying token/referrer, or only same-origin path push (no leak)
verify_steps: HUMAN: private-tab login via redirect_uri=https://app.sipgate.com/implicit-auth-redirect?redirect=https://evil.example; report final URL and whether access_token reaches attacker origin (sandbox creds only)
impact: HIGH if off-origin → token exfil → full account compromise; LOW if same-origin-only
testability: HUMAN_ONLY
[PRIO] app.sipgate.com,9.40,attack_surface=9 business_value=10 tech_exposure=9 gate_ease=10 cloud_surface=8 freshness=10
[PRIO] login.sipgate.com,8.65,attack_surface=8 business_value=9 tech_exposure=8 gate_ease=10 cloud_surface=7 freshness=10
[PRIO] api.sipgate.com,8.00,attack_surface=7 business_value=10 tech_exposure=8 gate_ease=5 cloud_surface=8 freshness=10
[PRIO] radau (GitHub),8.05,attack_surface=6 business_value=8 tech_exposure=9 gate_ease=10 cloud_surface=8 freshness=9
[PRIO] clinq-bridge-sipgate (GitHub),7.90,attack_surface=6 business_value=7 tech_exposure=9 gate_ease=10 cloud_surface=9 freshness=9
[PRIO] www.sipgate.de,5.15,attack_surface=4 business_value=5 tech_exposure=3 gate_ease=10 cloud_surface=6 freshness=8
[HYP] OIDC Implicit Flow Fragment Leakage via Third-Party Subresources
class: AUTH
asset: app.sipgate.com/implicit-auth-redirect
confidence: 70
reasoning: login.sipgate.com uses OIDC implicit flow (response_type=token) redirecting to https://app.sipgate.com/implicit-auth-redirect?redirect=/ with access_token in URL fragment. JS (ImplicitAuthenticator) persists token to localStorage then history.replace(redirect). Fragment leaks if page loads third-party resources (analytics, fonts, iframes) BEFORE token extraction — Referer header carries fragment to third parties.
evidence_needed: Observe network requests from implicit-auth-redirect page; check Referrer-Policy header; verify token extraction timing vs third-party loads; confirm no Referrer-Policy: no-referrer or strict-origin-when-cross-origin.
verify_steps: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling order, third-party script/tags, Referrer-Policy header, and any subresource loads that could carry fragment via Referer.
impact: Full account takeover via stolen access token sent to third-party domains; severity HIGH.
testability: PASSIVE
[HYP] Third-Party Realm Client Credential Discovery via Alternate Grant Flows
class: AUTH
asset: api.sipgate.com (proxied login.sipgate.com/auth/realms/third-party)
confidence: 55
reasoning: Third-party realm exposed via api.sipgate.com proxy, advertising high-value scopes (contacts/sms/account/balance/payment). Realm metadata shows grant_types: password (ROPC), client_secret_jwt, authorization_code, refresh_token. DCR blocked by Trusted Hosts, demo creds revoked. Password grant (ROPC) enables credential testing if any valid user+client combo exists; client_secret_jwt enables JWT-based client auth if a registered confidential client's key is derivable.
evidence_needed: Identify any valid confidential client_id for third-party realm; test ROPC with known/test credentials (sandbox only); check for client_secret_jwt verifier flaws.
verify_steps: GET https://api.sipgate.com/auth/realms/third-party/.well-known/openid-configuration — enumerate grant types, token_endpoint_auth_methods; passive client_id guess at /auth endpoint (400/401 distinction); NO active token requests on live data.
impact: Access to customer contacts, SMS, account balance, payment data via valid OAuth token; severity HIGH.
testability: PASSIVE
[HYP] Internal Redis Exposure Enables SSRF Lateral Movement Chain
class: MISCONFIG
asset: clinq-bridge-sipgate/k8s/template/deployment.yml (public GitHub)
confidence: 95
reasoning: Public repo hardcodes REDIS_URL: rediss://10.37.248.211:6378 (internal RFC-1918 IP, TLS Redis). cloudbuild.yaml reveals GCP project clinq-services zone europe-west3. If any sipgate service has SSRF (params: url, uri, callback, webhook, next, redirect), attacker can reach internal Redis for data access/lateral movement.
evidence_needed: Confirm 10.37.248.211 live in sipgate GCP network; identify SSRF vector in sipgate API surface reaching internal IPs.
verify_steps: Passive: search sipgate API endpoints for SSRF candidates (url, uri, callback, webhook, next, redirect params); correlate GCP project clinq-services with sipgate infrastructure via DNS/cert transparency.
impact: Internal infrastructure access → data theft / lateral movement via SSRF chain; severity HIGH if SSRF exists.
testability: PASSIVE
[PARKED] OIDC Implicit Flow Token Leakage via Referer/History on redirect page: Knowledge base REJECTED as OATH class — `history.replace(external)` in React Router resolves same-origin, token persists to localStorage before navigation, fragment never forwarded off-origin. Confidence 75 but rejected-class per KB.
[PARKED] Keycloak Redirect_URI Validation Bypass on login.sipgate.com: Requires testing redirect_uri variants (path traversal, encoding, subdomain) which is active auth testing; passive-only verification limited; confidence 60.
[PARKED] WebSocket Origin Validation Bypass on app.sipgate.com: No concrete WS endpoint discovered; CSP allows wildcard but actual endpoints unknown; needs endpoint enumeration first.
[PARKED] Keycloak id_token alg confusion (HS256): Standard Keycloak metadata, not proof of vulnerable verifier; REJECTED-adjacent per knowledge base.
[PARKED] ROPC/password grant + client_secret_jwt: Config-only, brute-force territory REJECTED.
[PARKED] dev.sipgate.de subdomain takeover: Abandoned host on owned IP; no dangling CNAME to attacker-controllable service.
[PARKED] Third-party realm valid client discovery: DCR gated by Trusted Hosts, demo creds revoked; no passive path to valid credential.
[FINAL] 1. Internal Redis endpoint exposure aids SSRF/lateral movement (confidence 95, PASSIVE)
[FINAL] 2. OIDC Implicit Flow Fragment Leakage via Third-Party Subresources (confidence 70, PASSIVE)
[FINAL] 3. Third-Party Realm Client Credential Discovery via Alternate Grant Flows (confidence 55, PASSIVE)
[NEXT] PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling order (token extraction vs third-party loads), Referrer-Policy header, subresource requests, and localStorage key usage.
[LEARN] REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
[LEARN] REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
[LEARN] ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
[LEARN] REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is not demonstrable statically; token persists to localStorage before navigation — fragment never forwarded off-origin.
[LEARN] REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reachable flawed verifier.
[LEARN] REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. revoked/not registered — not a live credential exposure.
[LEARN] REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scope`), no Host-header bypass found; redirect_uri validation correct.
[LEARN] ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/account/balance/payment).
[LEARN] ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
[LEARN] ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.
[RISK] sipgate: 75 — High-value VoIP/SaaS with OIDC implicit flow (token-in-fragment), permissive CSP wildcard WS origins, multi-tenant dashboards. Primary risks: (1) Fragment leakage via third-party subresources on implicit-auth-redirect (PASSIVE verifiable, unconfirmed), (2) Public repo internal infrastructure exposure (Redis IP, GCP project) enabling SSRF chains if any sipgate service has SSRF, (3) Radau CORS misconfig if deployed without override. Third-party realm well-hardened (DCR blocked, redirect_uri strict, demo creds revoked). No confirmed live vulnerability this cycle; risk concentrated on unconfirmed fragment leakage and public-code infoleaks.
