## 2026-09-03 17:19:01 UTC [target] (model bigpickle)
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
## 2026-09-03 20:01:16 UTC [target] (model bigpickle)
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
## 2026-09-03 22:31:36 UTC [target] (model bigpickle)
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
## 2026-09-04 00:39:44 UTC [target] (model bigpickle)
## 2026-09-04 05:09:12 UTC [target] (model bigpickle)
[HYP] Api-wide arbitrary-origin CORS reflection with credentials enables cross-origin customer-data disclosure when combined with a token-bearing client context
class: MISCONFIG
asset: api.sipgate.com/v2/* (confirmed on /contacts, /account, /numbers, /users, /authorization/userinfo)
confidence: 70
reasoning: every tested /v2 endpoint returns `Access-Control-Allow-Origin: <any supplied origin>` AND `Access-Control-Allow-Credentials: true` on both OPTIONS preflight and real responses, including `Origin: null` (sandboxed iframe) and arbitrary attacker origins; `authorization` is in allowed headers; `Access-Control-Expose-Headers` leaks location/x-sipgate-* on responses. Auth is Bearer-header (no Set-Cookie observed), so standalone auto-attach is NOT possible — exploitability requires a token/bearer context (e.g., XSS on app.sipgate.com where the implicit-flow token persists in localStorage, or a leaked bearer). Pairing the confirmed unvalidated client redirect + React SPA token-in-localStorage gives a plausible-but-unproven ATO chain.
evidence_needed: locate a client-side (app.sipgate.com) XSS/token-source to complete the chain, or confirm any API path authenticates via ambient cookie (would make CORS directly exploitable).
verify_steps: PASSIVE/active-confirmed — I observed Origin:null and arbitrary-origin reflection with credentials on GET and OPTIONS across 5+ endpoints; next: confirm whether app.sipgate.com sets/sends any cookie that the API honors, and locate a script-injection/token-leak vector on app.sipgate.com to complete ATO.
impact: cross-origin exfiltration of contacts/account/numbers/users/sms/balance if paired with a token source; standalone = CORS protection gap (no Origin allowlist), defense-in-depth. Severity: HIGH as chain, LOW-MEDIUM standalone.
testability: PASSIVE (CORS confirmed live); chain completion HUMAN_ONLY
## 2026-09-04 09:49:44 UTC [target] (model bigpickle)
[HYP] api.sipgate.com v2 CORS arbitrary-origin reflection enables cross-origin data exfiltration when paired with any token-bearing client context
class: MISCONFIG
asset: api.sipgate.com/v2/*
confidence: 70
reasoning: Confirmed: every tested /v2 endpoint returns Access-Control-Allow-Origin: <arbitrary> + Allow-Credentials: true on OPTIONS and GET. Origin:null reflected. Bearer auth only (no cookies). Chain: XSS on *.sipgate.com → read localStorage token → CORS exfiltrate contacts/SMS/account/balance/payments.
evidence_needed: XSS on any sipgate subdomain, or cookie-based auth on any API path
verify_steps: PASSIVE: enumerate swagger/graphql/additional endpoints, check CORS consistency, probe for unauthenticated endpoints
impact: full customer data exfiltration = CRITICAL; CORS gap alone = MEDIUM defense-in-depth
testability: PASSIVE (CORS confirmed); chain completion HUMAN_ONLY
[HYP] api.sipgate.com unauthenticated endpoint data leak via CORS
class: MISCONFIG
asset: api.sipgate.com (unauthenticated endpoints)
confidence: 45
reasoning: If any path accepts requests without Bearer auth and returns customer data, CORS reflection allows immediate exfiltration. Candidates: /health, /status, /config, /graphql, /.well-known/*.
evidence_needed: identify unauthenticated endpoint returning non-public data
verify_steps: PASSIVE: GET common paths, check CORS headers + response body
impact: immediate cross-origin data exfiltration = CRITICAL
testability: PASSIVE
[HYP] app.sipgate.com implicit flow token in localStorage enables ATO via XSS + CORS chain
class: OATH
asset: app.sipgate.com/implicit-auth-redirect
confidence: 45
reasoning: Token stored to localStorage before history.replace. If XSS on *.sipgate.com, attacker reads token + uses CORS to exfiltrate via api.sipgate.com.
evidence_needed: HUMAN browser confirmation of token storage; XSS on sipgate subdomain
verify_steps: HUMAN_ONLY: login via redirect, inspect localStorage/network
impact: full account compromise = CRITICAL
testability: HUMAN_ONLY
[PARKED] Implicit flow redirect (conf 45): needs HUMAN browser confirmation, not testable passively. Retained as top HUMAN lead.
[FINAL] survivors:
[NEXT] PROBE: GET https://api.sipgate.com/openapi.json + /swagger.json + /graphql + /health + /status + /.well-known/ + /v2/sms + /v2/calls + /v2/fax + /v2/subscription + /v2/payment each with Origin:https://evil.example — for each, check CORS headers + response body for sensitive data or auth errors. Also GET https://app.sipgate.com and scan JS bundles for 'pusher'+'channel'+'auth' configuration. Read-only, <=1 rps.
[LEARN] ACCEPTED MISCONFIG @ api.sipgate.com: confirmed arbitrary-origin CORS reflection with credentials on 5+ /v2 endpoints — strong defense-in-depth gap, chain-dependent for full exploitation.
[LEARN] REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: token stored before navigation, React Router same-origin — needs HUMAN browser confirmation.
[RISK] sipgate: 50 — Confirmed CORS arbitrary-origin reflection with credentials on api.sipgate.com is a real defense-in-depth gap; when paired with any XSS on *.sipgate.com or implicit flow token theft, enables full customer data exfiltration. Implicit flow unconfirmed (HUMAN_ONLY). No confirmed exploitable chain yet.
## 2026-09-04 14:13:27 UTC [target] (model bigpickle)
[NEW] `chatbot.dev.sipgate.com` — live dev chatbot (nginx/1.24.0) with socket.io endpoint accessible from internet, loads React dev builds from unpkg.com
[NEW] `chatbot.sipgate.com` — live production chatbot socket.io endpoint (`/chat/session/socket.io/`) accepts connections from any origin
[NEW] `payment.sipgate.com` — live payment API (Java Spring, JSESSIONID), proper CORS (only reflects `app.sipgate.com`)
[NEW] `team-de.live.sipgate.com` — live team portal (Apache/PHP), 302→login.sipgate.com; `frame-ancestors` whitelists `app.local.sipgate.com:3443` (local dev); `SERVERID=team-web03` leaked
[NEW] `app.dev.sipgate.com` — live dev SPA on Fastly CDN, serves identical main-C5_XLhfX.js bundle as production (no IP restriction)
[NEW] `api.dev.sipgate.com` — 403 Forbidden on all paths (WAF blocked, but accessible)
[NEW] Production JS bundle hardcodes internal dev URLs: `api.local.sipgate.com:3396`, `app.local.sipgate.com:3443`, `payment.local.sipgate.com:8080`, `team-de.local.sipgate.com:10443`, `login.dev.sipgate.com`
[NEW] `api.sipgate.com/health` — unauthenticated endpoint (200 OK, "Healthcheck - OK") with full arbitrary-origin CORS + credentials reflected
[CHANGED] `app.dev.sipgate.com` — CSP identical to production; JS references `api.dev.sipgate.com` in `pickByEnvironment` but dev API is 403-blocked
[CHANGED] `chatbot.dev.sipgate.com` — `/chat/session/socket.io/?EIO=4&transport=polling` returns valid socket.io session from any origin (no CORS check)
[PRIO] api.sipgate.com,9.0,attack_surface=9 business_value=10 tech_exposure=9 gate_ease=5 cloud_surface=8 freshness=10
[PRIO] app.dev.sipgate.com,8.5,attack_surface=9 business_value=8 tech_exposure=8 gate_ease=9 cloud_surface=8 freshness=10
[PRIO] chatbot.sipgate.com,8.0,attack_surface=7 business_value=9 tech_exposure=8 gate_ease=7 cloud_surface=7 freshness=10
[PRIO] team-de.live.sipgate.com,7.5,attack_surface=6 business_value=8 tech_exposure=7 gate_ease=6 cloud_surface=6 freshness=10
[PRIO] payment.sipgate.com,7.0,attack_surface=5 business_value=10 tech_exposure=6 gate_ease=4 cloud_surface=6 freshness=10
[PRIO] chatbot.dev.sipgate.com,7.0,attack_surface=6 business_value=7 tech_exposure=7 gate_ease=8 cloud_surface=7 freshness=10
[PRIO] app.sipgate.com,7.5,attack_surface=8 business_value=10 tech_exposure=8 gate_ease=5 cloud_surface=7 freshness=10
[PRIO] login.sipgate.com,7.0,attack_surface=7 business_value=9 tech_exposure=8 gate_ease=5 cloud_surface=6 freshness=10
[HYP] app.dev.sipgate.com serves identical production JS bundle with hardcoded dev/local URLs enabling internal infrastructure discovery
class: MISCONFIG
asset: app.dev.sipgate.com
confidence: 75
reasoning: app.dev.sipgate.com serves the same main-C5_XLhfX.js as production. `pickByEnvironment` is hardcoded with `local` URLs (api.local:3396, app.local:3443, payment.local:8080, team-de.local:10443, team-uk.local:10443, login.dev, chatbot.dev). Dev SPA is publicly accessible on Fastly CDN with no IP restriction, identical CSP, and full SPA functionality. The dev environment reveals internal hostnames, port numbers, and service architecture.
evidence_needed: confirm dev app.sipgate.com can authenticate users (login.dev.sipgate.com Keycloak realm) or find additional internal endpoints exposed via dev JS; confirm whether dev API (api.dev.sipgate.com 403) can be bypassed.
verify_steps: GET https://app.dev.sipgate.com and compare CSP/JS bundle hash vs production; GET https://app.dev.sipgate.com/app-login to check if dev login works; GET https://login.dev.sipgate.com/auth/realms/sipgate-apps/.well-known/openid-configuration for dev Keycloak config.
impact: information disclosure of internal infrastructure (hostnames, ports, service mesh) enabling targeted SSRF/lateral movement; dev environment may have weaker auth controls = potential full account takeover. Severity: MEDIUM (info leak) to HIGH (if dev auth bypassable).
testability: PASSIVE
[HYP] chatbot.sipgate.com production socket.io accepts cross-origin connections without authentication enabling session hijack
class: AUTH
asset: chatbot.sipgate.com/chat/session/socket.io
confidence: 60
reasoning: Production chatbot at chatbot.sipgate.com returns 200 OK with valid socket.io session ID (`sid`) on `/chat/session/socket.io/?EIO=4&transport=polling` from any Origin (evil.example confirmed). The production JS uses `getToken()` to obtain bearer token for socket auth, but the socket.io transport layer itself accepts connections from arbitrary origins. If the socket.io server validates auth only on specific namespaces/events but accepts the initial handshake from any origin, an attacker-controlled page could establish a socket.io connection and potentially intercept chat messages or inject commands.
evidence_needed: HUMAN browser: connect to chatbot.sipgate.com socket.io from attacker origin, observe whether auth token is required for message events or only at connection time; check if socket sends user data before auth validation.
verify_steps: PASSIVE: GET /chat/session/socket.io/?EIO=4&transport=polling with Origin:https://evil.example — confirm session established; check for auth-related socket events in response.
impact: session hijack of customer chat sessions = PII exposure; if admin/support chat = lateral movement. Severity: HIGH.
testability: PASSIVE (socket handshake confirmed); full chain HUMAN_ONLY
[HYP] team-de.live.sipgate.com CSP frame-ancestors whitelist includes local dev origins enabling clickjacking/CSRF on team management portal
class: MISCONFIG
asset: team-de.live.sipgate.com
confidence: 65
reasoning: team-de.live.sipgate.com sets `frame-ancestors 'self' *.sipgate.com *.sipgate.de *.sipgate.co.uk app.local.sipgate.com:3443`. The inclusion of `app.local.sipgate.com:3443` (internal dev origin) in the production CSP frame-ancestors is a misconfiguration — any sipgate employee running the dev app locally can frame the production team portal. The portal handles authentication (302→login.sipgate.com), team management, and user administration. Also leaks `SERVERID=team-web03` cookie (internal server naming). The portal is Apache/PHP with JSESSIONID-less session management.
evidence_needed: HUMAN: test if `app.local.sipgate.com:3443` origin can frame team-de.live.sipgate.com in a browser; check if authenticated team operations can be CSRF-triggered via the framed page.
verify_steps: PASSIVE: GET team-de.live.sipgate.com with Origin header; inspect CSP frame-ancestors; check for CSRF tokens in forms.
impact: clickjacking on team management portal → unauthorized team changes; info disclosure of internal server naming. Severity: MEDIUM.
testability: PASSIVE (CSP leak confirmed); exploitation HUMAN_ONLY
[PARKED] api.sipgate.com /health unauthenticated CORS reflection: /health returns only "Healthcheck - OK" — no customer data, not exploitable standalone. Downgraded from hypothesis to confirmed observation. Retained as chain component.
[PARKED] api.sipgate.com CORS arbitrary-origin (conf 70): KB ACCEPTED; already confirmed. Carry forward as chain component for new hypotheses.
[PARKED] app.sipgate.com implicit flow redirect (conf 45): KB REJECTED OATH — needs HUMAN browser confirmation, not testable passively. Retained as top HUMAN lead.
[PARKED] chatbot.dev.sipgate.com socket.io: dev environment is publicly accessible but socket.io without auth tokens is likely non-functional demo. Lower value than production chatbot endpoint.
[PARKED] payment.sipgate.com: proper CORS (only reflects app.sipgate.com), 401 on unauthenticated — hardened, not exploitable.
[FINAL] survivors:
[NEXT] PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check `/.well-known/openid-configuration` on login.dev.sipgate.com). Also GET https://app.dev.sipgate.com with Origin:https://evil.example to check for any CORS headers. Also GET https://chatbot.sipgate.com/chat/session/socket.io/?EIO=4&transport=polling with Origin:https://evil.example and check `vary` header for CORS behavior. Read-only, <=1 rps.
[LEARN] ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs (api.local:3396, app.local:3443, payment.local:8080, team-de.local:10443), no IP restriction — infrastructure info disclosure.
[LEARN] ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes `app.local.sipgate.com:3443` (internal dev origin) in production portal; leaks `SERVERID=team-web03`.
[LEARN] ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
[LEARN] ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
[RISK] sipgate: 55 — Confirmed defense-in-depth gaps: (1) arbitrary-origin CORS reflection with credentials on api.sipgate.com/v2/* + /health (KB ACCEPTED), (2) publicly accessible dev SPA with hardcoded internal infrastructure URLs (NEW ACCEPTED), (3) production chatbot socket.io accepting cross-origin connections (NEW ACCEPTED), (4) team portal CSP misconfiguration exposing local dev origin (NEW ACCEPTED). Chain-dependent for full exploitation: api.sipgate.com CORS + any XSS on *.sipgate.com + implicit flow token in localStorage = full customer data exfiltration (CRITICAL). Implicit flow redirect unconfirmed (HUMAN_ONLY). No confirmed exploitable standalone chain yet.
## 2026-09-04 17:43:47 UTC [target] (model bigpickle)
[PRIO] api.sipgate.com,9.0,attack_surface=9 business_value=10 tech_exposure=9 gate_ease=5 cloud_surface=8 freshness=10
[PRIO] app.dev.sipgate.com,8.5,attack_surface=9 business_value=8 tech_exposure=8 gate_ease=9 cloud_surface=8 freshness=10
[PRIO] chatbot.sipgate.com,8.0,attack_surface=7 business_value=9 tech_exposure=8 gate_ease=7 cloud_surface=7 freshness=10
[PRIO] team-de.live.sipgate.com,7.5,attack_surface=6 business_value=8 tech_exposure=7 gate_ease=6 cloud_surface=6 freshness=10
[PRIO] payment.sipgate.com,7.0,attack_surface=5 business_value=10 tech_exposure=6 gate_ease=4 cloud_surface=6 freshness=10
[PRIO] chatbot.dev.sipgate.com,7.0,attack_surface=6 business_value=7 tech_exposure=7 gate_ease=8 cloud_surface=7 freshness=10
[PRIO] app.sipgate.com,7.5,attack_surface=8 business_value=10 tech_exposure=8 gate_ease=5 cloud_surface=7 freshness=10
[PRIO] login.sipgate.com,7.0,attack_surface=7 business_value=9 tech_exposure=8 gate_ease=5 cloud_surface=6 freshness=10
[HYP] app.dev.sipgate.com serves identical production JS bundle with hardcoded dev/local URLs enabling internal infrastructure discovery
class: MISCONFIG
asset: app.dev.sipgate.com
confidence: 75
reasoning: app.dev.sipgate.com serves the same main-C5_XLhfX.js as production. `pickByEnvironment` is hardcoded with `local` URLs (api.local:3396, app.local:3443, payment.local:8080, team-de.local:10443, team-uk.local:10443, login.dev, chatbot.dev). Dev SPA is publicly accessible on Fastly CDN with no IP restriction, identical CSP, and full SPA functionality. The dev environment reveals internal hostnames, port numbers, and service architecture.
evidence_needed: confirm dev app.sipgate.com can authenticate users (login.dev.sipgate.com Keycloak realm) or find additional internal endpoints exposed via dev JS; confirm whether dev API (api.dev.sipgate.com 403) can be bypassed.
verify_steps: GET https://app.dev.sipgate.com and compare CSP/JS bundle hash vs production; GET https://app.dev.sipgate.com/app-login to check if dev login works; GET https://login.dev.sipgate.com/auth/realms/sipgate-apps/.well-known/openid-configuration for dev Keycloak config.
impact: information disclosure of internal infrastructure (hostnames, ports, service mesh) enabling targeted SSRF/lateral movement; dev environment may have weaker auth controls = potential full account takeover. Severity: MEDIUM (info leak) to HIGH (if dev auth bypassable).
testability: PASSIVE
[HYP] chatbot.sipgate.com production socket.io accepts cross-origin connections without authentication enabling session hijack
class: AUTH
asset: chatbot.sipgate.com/chat/session/socket.io
confidence: 60
reasoning: Production chatbot at chatbot.sipgate.com returns 200 OK with valid socket.io session ID (`sid`) on `/chat/session/socket.io/?EIO=4&transport=polling` from any Origin (evil.example confirmed). The production JS uses `getToken()` to obtain bearer token for socket auth, but the socket.io transport layer itself accepts connections from arbitrary origins. If the socket.io server validates auth only on specific namespaces/events but accepts the initial handshake from any origin, an attacker-controlled page could establish a socket.io connection and potentially intercept chat messages or inject commands.
evidence_needed: HUMAN browser: connect to chatbot.sipgate.com socket.io from attacker origin, observe whether auth token is required for message events or only at connection time; check if socket sends user data before auth validation.
verify_steps: PASSIVE: GET /chat/session/socket.io/?EIO=4&transport=polling with Origin:https://evil.example — confirm session established; check for auth-related socket events in response.
impact: session hijack of customer chat sessions = PII exposure; if admin/support chat = lateral movement. Severity: HIGH.
testability: PASSIVE (socket handshake confirmed); full chain HUMAN_ONLY
[HYP] team-de.live.sipgate.com CSP frame-ancestors whitelist includes local dev origins enabling clickjacking/CSRF on team management portal
class: MISCONFIG
asset: team-de.live.sipgate.com
confidence: 65
reasoning: team-de.live.sipgate.com sets `frame-ancestors 'self' *.sipgate.com *.sipgate.de *.sipgate.co.uk app.local.sipgate.com:3443`. The inclusion of `app.local.sipgate.com:3443` (internal dev origin) in the production CSP frame-ancestors is a misconfiguration — any sipgate employee running the dev app locally can frame the production team portal. The portal handles authentication (302→login.sipgate.com), team management, and user administration. Also leaks `SERVERID=team-web03` cookie (internal server naming). The portal is Apache/PHP with JSESSIONID-less session management.
evidence_needed: HUMAN: test if `app.local.sipgate.com:3443` origin can frame team-de.live.sipgate.com in a browser; check if authenticated team operations can be CSRF-triggered via the framed page.
verify_steps: PASSIVE: GET team-de.live.sipgate.com with Origin header; inspect CSP frame-ancestors; check for CSRF tokens in forms.
impact: clickjacking on team management portal → unauthorized team changes; info disclosure of internal server naming. Severity: MEDIUM.
testability: PASSIVE (CSP leak confirmed); exploitation HUMAN_ONLY
[PARKED] api.sipgate.com /health unauthenticated CORS reflection: /health returns only "Healthcheck - OK" — no customer data, not exploitable standalone. Downgraded from hypothesis to confirmed observation. Retained as chain component.
[PARKED] api.sipgate.com CORS arbitrary-origin (conf 70): KB ACCEPTED; already confirmed. Carry forward as chain component for new hypotheses.
[PARKED] app.sipgate.com implicit flow redirect (conf 45): KB REJECTED OATH — needs HUMAN browser confirmation, not testable passively. Retained as top HUMAN lead.
[PARKED] chatbot.dev.sipgate.com socket.io: dev environment is publicly accessible but socket.io without auth tokens is likely non-functional demo. Lower value than production chatbot endpoint.
[PARKED] payment.sipgate.com: proper CORS (only reflects app.sipgate.com), 401 on unauthenticated — hardened, not exploitable.
[FINAL] survivors:
[NEXT] PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check `/.well-known/openid-configuration` on login.dev.sipgate.com). Also GET https://app.dev.sipgate.com with Origin:https://evil.example to check for any CORS headers. Also GET https://chatbot.sipgate.com/chat/session/socket.io/?EIO=4&transport=polling with Origin:https://evil.example and check `vary` header for CORS behavior. Read-only, <=1 rps.
[LEARN] ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs (api.local:3396, app.local:3443, payment.local:8080, team-de.local:10443), no IP restriction — infrastructure info disclosure.
[LEARN] ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes `app.local.sipgate.com:3443` (internal dev origin) in production portal; leaks `SERVERID=team-web03`.
[LEARN] ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
[LEARN] ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
[RISK] sipgate: 55 — Confirmed defense-in-depth gaps: (1) arbitrary-origin CORS reflection with credentials on api.sipgate.com/v2/* + /health (KB ACCEPTED), (2) publicly accessible dev SPA with hardcoded internal infrastructure URLs (NEW ACCEPTED), (3) production chatbot socket.io accepting cross-origin connections (NEW ACCEPTED), (4) team portal CSP misconfiguration exposing local dev origin (NEW ACCEPTED). Chain-dependent for full exploitation: api.sipgate.com CORS + any XSS on *.sipgate.com + implicit flow token in localStorage = full customer data exfiltration (CRITICAL). Implicit flow redirect unconfirmed (HUMAN_ONLY). No confirmed exploitable standalone chain yet.
## 2026-09-04 19:59:36 UTC [target] (model bigpickle)
## 2026-09-04 22:20:59 UTC [target] (model bigpickle)
[PRIO] api.sipgate.com,9.0,attack_surface=9 business_value=10 tech_exposure=9 gate_ease=5 cloud_surface=8 freshness=10
[PRIO] sipgate-desktop-app.s3,7.0,attack_surface=4 business_value=7 tech_exposure=5 gate_ease=10 cloud_surface=9 freshness=10
[PRIO] chatbot.sipgate.com,7.0,attack_surface=6 business_value=8 tech_exposure=7 gate_ease=6 cloud_surface=6 freshness=10
[PRIO] app.dev.sipgate.com,6.7,attack_surface=6 business_value=5 tech_exposure=7 gate_ease=9 cloud_surface=7 freshness=8
[PRIO] static.sipgate.com,5.9,attack_surface=5 business_value=4 tech_exposure=5 gate_ease=10 cloud_surface=6 freshness=8
[HYP] api.sipgate.com BOLA on multi-tenant resources with authz drift across stale-documented endpoints
class: IDOR
asset: api.sipgate.com/v2/{portings,history,addresses,devices,groups}
confidence: 45
reasoning: Public swagger (144 paths) documents dual/inconsistent parameter names (`/contacts/{contactId}` AND `/contacts/{contactid}`), renames legacy resources (`/log/webhooks`, `/app/tacs`, `/app/properties`, `/settings/sipgateio`), and 5 ops marked noauth (app/links, userinfo, gdpr) — none actually work unauthenticated → security annotations are stale, implying per-endpoint authz drift on a many-year-old API surface. Confirmed accessible unauthenticated: only /translations/{language} and /swagger.json.
evidence_needed: two authenticated tenant sessions showing 200 for a resourceId that belongs to the other tenant on /portings/{portingId}, /history/{entryId}/note, /addresses/{addressId}/numbers, /devices/{deviceId}/credentials/password.
verify_steps: PASSIVE triage GET /v2/portings, /v2/log/webhooks, /v2/app/tacs, /v2/settings/sipgateio without auth — flag any non-401/-404/-405; full cross-tenant test requires AUTH_HELPED sessions.
impact: cross-tenant PII/call-history/device-SIP-credential exposure or overwrite. Severity: CRITICAL if confirmed.
testability: AUTH_HELPED
[HYP] Publicly listable desktop-app S3 bucket is a supply-chain distribution surface
class: MISCONFIG
asset: sipgate-desktop-app.s3.eu-central-1.amazonaws.com
confidence: 60
reasoning: ls buckets — GET / returns ListBucketResult of all installer artifacts (1.3.0–1.17.19, per-OS, blockmaps, latest alias objects); ACL/policy reads 403 (config = intended public read). Last write 2024-06-11 → artifact stale 2+ yrs. If ANY write path exists (unverified, untested), installer poisoning → full softphone compromise.
evidence_needed: ability to PUT/OVERWRITE an object (NOT tested — mutating supply-chain operation).
verify_steps: PASSIVE list already done; write-check is HUMAN with reporter/legal sign-off only.
impact: OSINT of release cadence + artifact integrity surface; CRITICAL chain if writable. Standalone severity: LOW-MED.
testability: PASSIVE
[HYP] chatbot.sipgate.com WebSocket transport enables cross-origin chat interception
class: AUTH
asset: chatbot.sipgate.com/chat/session/socket.io
confidence: 50
reasoning: Socket.io polling handshake serves 200 + vary:Origin but no ACAO for arbitrary origins (browser blocks reads on polling). WebSocket transport (EIO=4&transport=websocket) bypasses CORS entirely; server-side Origin check on WS upgrade unconfirmed. If chat events are broadcast to sockets without per-event auth, cross-origin socket = live chat read.
evidence_needed: HUMAN browser page on evil origin opening wss to chatbot.sipgate.com and observing 40/auth/chat events.
verify_steps: PASSIVE handshake confirmed earlier (KB ACCEPTED); WS upgrade behavior needs HUMAN.
impact: read/intercept customer chat sessions → PII. Severity: HIGH if confirmed.
testability: HUMAN_ONLY
[PARKED] app.dev.sipgate.com dev-auth ATO hypothesis: login.dev.sipgate.com HTTP 000 + api.dev 403 → dev env externally inert; only static info-leak (already ACCEPTED) stands. Confidence dropped <40.
[PARKED] chatbot.sipgate.com polling cross-origin read: no ACAO → browser-blocks reads; narrowed to WS-transport-only (subsumed into H3).
[PARKED] api.sipgate.com CORS reflection: re-confirmed again on 404s + /v2/app/links; parked chain component.
[PARKED] app.sipgate.com implicit-flow redirect: unchanged, KB REJECTED OATH, HUMAN-only.
[FINAL] survivors: H1 api BOLA (45, AUTH_HELPED); H2 s3 bucket (60, PASSIVE chain); H3 chatbot WS (50, HUMAN_ONLY).
[NEXT] PROBE: 4 read-only GETs @1rps against unusual high-value swagger paths for authz-drift triage: https://api.sipgate.com/v2/portings, https://api.sipgate.com/v2/log/webhooks, https://api.sipgate.com/v2/app/tacs, https://api.sipgate.com/v2/settings/sipgateio — flag any status other than 401/404/405.
[LEARN] ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + latest aliases, stale since 2024-06-11); ACL/policy reads denied; write path NOT tested (mutating, not performed).
[LEARN] REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path deflated to static info-leak only.
[LEARN] CHANGED AUTH @ chatbot.sipgate.com: polling transport serves vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbitrary origins" narrowed to WS transport (HUMAN_ONLY).
[LEARN] ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior — authz-drift triage input.
[RISK] sipgate: 55 — Confirmed: api CORS reflection w/ credentials, public dev-SPA info leak (inert auth), team portal CSP/local-origin leak, chatbot WS (HUMAN), and NEW publicly listable desktop-app S3 bucket (supply-chain surface, write path untested). All standalone items are defense-in-depth/info; full impact still chain-dependent (api CORS + XSS + implicit-flow localStorage, or BOLA on v2 requiring creds). No confirmed standalone critical chain yet.
## 2026-09-05 00:18:40 UTC [target] (model bigpickle)
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
[PRIO] app.dev.sipgate.com,6.7,attack_surface=6 business_value=5 tech_exposure=7 gate_ease=9 cloud_surface=7 freshness=8
[PRIO] static.sipgate.com,5.9,attack_surface=5 business_value=4 tech_exposure=5 gate_ease=10 cloud_surface=6 freshness=8
[HYP] api.sipgate.com BOLA on multi-tenant resources with authz drift across stale-documented endpoints
class: IDOR
asset: api.sipgate.com/v2/{portings,history,addresses,devices,groups}
confidence: 45
reasoning: Public swagger (144 paths) documents dual/inconsistent parameter names (`/contacts/{contactId}` AND `/contacts/{contactid}`), renames legacy resources (`/log/webhooks`, `/app/tacs`, `/app/properties`, `/settings/sipgateio`), and 5 ops marked noauth (app/links, userinfo, gdpr) — none actually work unauthenticated → security annotations are stale, implying per-endpoint authz drift on a many-year-old API surface. Confirmed accessible unauthenticated: only /translations/{language} and /swagger.json.
evidence_needed: two authenticated tenant sessions showing 200 for a resourceId that belongs to the other tenant on /portings/{portingId}, /history/{entryId}/note, /addresses/{addressId}/numbers, /devices/{deviceId}/credentials/password.
verify_steps: PASSIVE triage GET /v2/portings, /v2/log/webhooks, /v2/app/tacs, /v2/settings/sipgateio without auth — flag any non-401/-404/-405; full cross-tenant test requires AUTH_HELPED sessions.
impact: cross-tenant PII/call-history/device-SIP-credential exposure or overwrite. Severity: CRITICAL if confirmed.
testability: AUTH_HELPED
[HYP] Publicly listable desktop-app S3 bucket is a supply-chain distribution surface
class: MISCONFIG
asset: sipgate-desktop-app.s3.eu-central-1.amazonaws.com
confidence: 60
reasoning: ls buckets — GET / returns ListBucketResult of all installer artifacts (1.3.0–1.17.19, per-OS, blockmaps, latest alias objects); ACL/policy reads 403 (config = intended public read). Last write 2024-06-11 → artifact stale 2+ yrs. If ANY write path exists (unverified, untested), installer poisoning → full softphone compromise.
evidence_needed: ability to PUT/OVERWRITE an object (NOT tested — mutating supply-chain operation).
verify_steps: PASSIVE list already done; write-check is HUMAN with reporter/legal sign-off only.
impact: OSINT of release cadence + artifact integrity surface; CRITICAL chain if writable. Standalone severity: LOW-MED.
testability: PASSIVE
[HYP] chatbot.sipgate.com WebSocket transport enables cross-origin chat interception
class: AUTH
asset: chatbot.sipgate.com/chat/session/socket.io
confidence: 50
reasoning: Socket.io polling handshake serves 200 + vary:Origin but no ACAO for arbitrary origins (browser blocks reads on polling). WebSocket transport (EIO=4&transport=websocket) bypasses CORS entirely; server-side Origin check on WS upgrade unconfirmed. If chat events are broadcast to sockets without per-event auth, cross-origin socket = live chat read.
evidence_needed: HUMAN browser page on evil origin opening wss to chatbot.sipgate.com and observing 40/auth/chat events.
verify_steps: PASSIVE handshake confirmed earlier (KB ACCEPTED); WS upgrade behavior needs HUMAN.
impact: read/intercept customer chat sessions → PII. Severity: HIGH if confirmed.
testability: HUMAN_ONLY
[PARKED] app.dev.sipgate.com dev-auth ATO hypothesis: login.dev.sipgate.com HTTP 000 + api.dev 403 → dev env externally inert; only static info-leak (already ACCEPTED) stands. Confidence dropped <40.
[PARKED] chatbot.sipgate.com polling cross-origin read: no ACAO → browser-blocks reads; narrowed to WS-transport-only (subsumed into H3).
[PARKED] api.sipgate.com CORS reflection: re-confirmed again on 404s + /v2/app/links; parked chain component.
[PARKED] app.sipgate.com implicit-flow redirect: unchanged, KB REJECTED OATH, HUMAN-only.
[FINAL] survivors: H1 api BOLA (45, AUTH_HELPED); H2 s3 bucket (60, PASSIVE chain); H3 chatbot WS (50, HUMAN_ONLY).
[NEXT] PROBE: 4 read-only GETs @1rps against unusual high-value swagger paths for authz-drift triage: https://api.sipgate.com/v2/portings, https://api.sipgate.com/v2/log/webhooks, https://api.sipgate.com/v2/app/tacs, https://api.sipgate.com/v2/settings/sipgateio — flag any status other than 401/404/405.
[LEARN] ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + latest aliases, stale since 2024-06-11); ACL/policy reads denied; write path NOT tested (mutating, not performed).
[LEARN] REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path deflated to static info-leak only.
[LEARN] CHANGED AUTH @ chatbot.sipgate.com: polling transport serves vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbitrary origins" narrowed to WS transport (HUMAN_ONLY).
[LEARN] ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior — authz-drift triage input.
[RISK] sipgate: 55 — Confirmed: api CORS reflection w/ credentials, public dev-SPA info leak (inert auth), team portal CSP/local-origin leak, chatbot WS (HUMAN), and NEW publicly listable desktop-app S3 bucket (supply-chain surface, write path untested). All standalone items are defense-in-depth/info; full impact still chain-dependent (api CORS + XSS + implicit-flow localStorage, or BOLA on v2 requiring creds). No confirmed standalone critical chain yet.
[LEARN] REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path (oauth2/clients, userinfo, app/links, users, contacts csv/internal, history/export, portings, per-id paths) returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confirmed (noauth ops are 401), but server authz is uniformly enforced at edge -> authz-drift/BOLA not reachable unauthenticated; requires AUTH_HELPED tenant pairs.
[LEARN] REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return the same 200 English dict (whitelist-with-fallback) -> no LFI/traversal; inert endpoint, KB INFO stands only.
[LEARN] ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-request tracing middleware.
[RISK] sipgate: 55 — unchanged. Pending-probe set returned null (all 401), translations inert, no new confirmed vuln; BOLA H1 unchanged (45, AUTH_HELPED) — api.sipgate.com requires credentialed tenant-pair testing to advance, or chain via confirmed CORS reflection + any *.sipgate.com XSS (none found) + implicit-flow localStorage token.
[HYP] api.sipgate.com BOLA on multi-tenant resources (server auth enforces 401 unauth, cross-tenant boundary untested)
class: IDOR
asset: api.sipgate.com/v2/{portings/{portingId},history/{entryId}/note,addresses/{addressId}/numbers,devices/{deviceId}/credentials/password}
confidence: 45
reasoning: Today's triage confirms the 401-vs-404 distinction separates authed-existing (401) from unknown (404) — all 14 documented paths incl dual-cased `/contacts/{contactId}` vs `/contacts/{contactid}` are auth-gated, so stale swagger annotations do not imply missing edge auth. The multi-tenant authorization boundary (tenant A's authenticated session hitting tenant B's resourceId) remains entirely unexercised; swagger documents per-resource credential-bearing responses (`addresses/{id}/numbers`, `{userId}/phonelines/{phonelineId}/sipgateio`, `devices/{id}/credentials`), which is the CRITICAL-class prize.
evidence_needed: two authenticated tenant sessions; tenant A's token returns 200 (not 403/404) on tenant B's `{portingId}`, `{addressId}`, `{deviceId}`, `{userId}` resources.
verify_steps: AUTH_HELPED only. Passive exhausted: all unauth probes 401; next step is credentialed tenant-pair differential testing on test/sandbox accounts.
impact: cross-tenant PII, call history, device SIP credentials, sipgate.io credentials exposure/overwrite. Severity: CRITICAL if confirmed.
testability: AUTH_HELPED
[HYP] chatbot.sipgate.com WebSocket transport enables cross-origin chat interception
class: AUTH
asset: chatbot.sipgate.com/chat/session/socket.io (transport=websocket)
confidence: 50
reasoning: Polling handshake = 200 + vary:Origin, no ACAO → browser-blocks polling reads (KB CHANGED). WS transport bypasses CORS; server-side Origin check on the WS upgrade is unverified. If chat events broadcast without per-event auth, a cross-origin socket reads live chat.
evidence_needed: HUMAN browser on evil origin opening wss and observing 40/auth/chat events.
verify_steps: PASSIVE handshake done (KB ACCEPTED); WS upgrade requires HUMAN.
impact: live customer chat read/intercept → PII. Severity: HIGH if confirmed.
testability: HUMAN_ONLY
[HYP] Publicly listable desktop-app S3 bucket as supply-chain distribution surface
class: MISCONFIG
asset: sipgate-desktop-app.s3.eu-central-1.amazonaws.com
confidence: 60
reasoning: GET / returns ListBucketResult (1.3.0–1.17.19, per-OS, blockmaps, latest aliases); last write 2024-06-11; ACL/policy reads 403 (intended public read). Write path never tested (mutating; requires sign-off).
evidence_needed: PUT/OVERWRITE ability (HUMAN + legal/reporter sign-off, not performed).
verify_steps: list done; write-check is HUMAN-only.
impact: installer poisoning → full softphone compromise if writable. Standalone severity: LOW-MED; CRITICAL if writable confirmed.
testability: PASSIVE (chain)
