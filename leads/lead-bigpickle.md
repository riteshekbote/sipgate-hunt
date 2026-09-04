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
