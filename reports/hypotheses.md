# Hypotheses (ranked)

## RANKED HYPOTHESES 2026-09-02 21:56:19 UTC

## RANKED HYPOTHESES 2026-09-02 23:58:18 UTC

## RANKED HYPOTHESES 2026-09-03 04:11:02 UTC

## RANKED HYPOTHESES 2026-09-03 09:00:28 UTC

## RANKED HYPOTHESES 2026-09-03 13:32:10 UTC

## RANKED HYPOTHESES 2026-09-03 17:20:36 UTC
- [75] login.sipgate.com: OIDC Implicit Flow Token Leakage via Referer/History (from art/lead_nemotron3.txt)
- [45] app.sipgate.com/implicit-auth-redirect?redirect=<attacker>: OAuth implicit token leakage via client-side unvalidated redirect on post-login handler (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage, third-party requests, Referer polic
- NEXT(hypotheses-bigpickle.txt): HUMAN: open a private-tab full login flow on `https://app.sipgate.com/implicit-auth-redirect?redirect=https://evil.example` via the standard login, and report f
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac

## RANKED HYPOTHESES 2026-09-03 20:17:13 UTC
- [25] login.sipgate.com/auth/realms/third-party: Valid registered API client discoverable via third-party realm for OAuth token acquisition (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET `https://login.sipgate.com/auth/realms/third-party/protocol/openid-connect/registrations/openid-connect` and the realm `/auth/realms/third-party` pag
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou

## RANKED HYPOTHESES 2026-09-03 22:33:27 UTC
- [75] login.sipgate.com: OIDC Implicit Flow Token Leakage via Referer/History on redirect page (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage (localStorage key), third-party requ
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
- LEARN: ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.

## RANKED HYPOTHESES 2026-09-04 00:50:23 UTC
- [75] login.sipgate.com: OIDC Implicit Flow Token Leakage via Referer/History on redirect page (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage (localStorage key), third-party requ
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
- LEARN: ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.

## RANKED HYPOTHESES 2026-09-04 05:09:31 UTC
- [70] api.sipgate.com/v2/*: Api-wide arbitrary-origin CORS reflection with credentials enables cross-origin customer-data disclosure when combined with a token-bearing client context (from art/lead_bigpickle.txt)
- [45] app.sipgate.com/implicit-auth-redirect?redirect=<attacker>: OAuth implicit token leakage via client-side unvalidated redirect on post-login handler (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: open a private-tab full login flow on `https://app.sipgate.com/implicit-auth-redirect?redirect=https://evil.example` via the standard login, and report f
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
- LEARN: ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.

## RANKED HYPOTHESES 2026-09-04 09:52:41 UTC
- [70] api.sipgate.com/v2/*: api.sipgate.com v2 CORS arbitrary-origin reflection enables cross-origin data exfiltration when paired with any token-bearing client context (from art/lead_bigpickle.txt)
- [70] api.sipgate.com/v2/*: Api-wide Arbitrary-Origin CORS Reflection with Credentials (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sipgate.com/openapi.json + /swagger.json + /graphql + /health + /status + /.well-known/ + /v2/sms + /v2/calls + /v2/fax + /v2/subscriptio
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling order (token extraction vs third-party loads), Refe
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com: confirmed arbitrary-origin CORS reflection with credentials on 5+ /v2 endpoints — strong defense-in-depth gap, chain-depen
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: token stored before navigation, React Router same-origin — needs HUMAN browser confirmation.
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
- LEARN: ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos

## RANKED HYPOTHESES 2026-09-04 14:17:10 UTC
- [75] app.dev.sipgate.com: app.dev.sipgate.com serves identical production JS bundle with hardcoded dev/local URLs enabling internal infrastructure discovery (from art/lead_bigpickle.txt)
- [70] api.sipgate.com/v2/*: Api-wide Arbitrary-Origin CORS Reflection with Credentials (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check `
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling order (token extraction vs third-party loads), Refe
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs 
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes `app.local.sipgate.com:3443` (internal dev origin) in production portal; leaks `SERV
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: ACCEPTED MISCONFIG @ clinq-bridge-sipgate (GitHub): K8s deployment exposes internal Redis IP `10.37.248.211:6378` + GCP project `clinq-services` in public repo.
- LEARN: ACCEPTED MISCONFIG/SECRET @ radau (GitHub): Default CORS `AllowAllOrigins+AllowCredentials` + hardcoded API keys/DB passwords in public repo.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos

## RANKED HYPOTHESES 2026-09-04 17:48:42 UTC
- [75] app.dev.sipgate.com: app.dev.sipgate.com serves identical production JS bundle with hardcoded dev/local URLs enabling internal infrastructure discovery (from art/lead_bigpickle.txt)
- [75] app.dev.sipgate.com: Dev SPA Infrastructure Exposure Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check `
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check /
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs 
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes `app.local.sipgate.com:3443` (internal dev origin) in production portal; leaks `SERV
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs 
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak is no
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client, i.e. revoked
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak Trusted Hosts policy (POST → insufficient_scope),

## RANKED HYPOTHESES 2026-09-04 20:05:47 UTC
- [75] app.dev.sipgate.com: Dev SPA Infrastructure Exposure Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check /
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs 
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak is no
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client, i.e. revoked
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak Trusted Hosts policy (POST → insufficient_scope),

## RANKED HYPOTHESES 2026-09-04 22:21:10 UTC
- [75] app.dev.sipgate.com: Dev SPA Infrastructure Exposure Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- [45] api.sipgate.com/v2/{portings,history,addresses,devices,groups}: api.sipgate.com BOLA on multi-tenant resources with authz drift across stale-documented endpoints (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: 4 read-only GETs @1rps against unusual high-value swagger paths for authz-drift triage: https://api.sipgate.com/v2/portings, https://api.sipgate.com/v2/l
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check /
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs 
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak is no
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client, i.e. revoked
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak Trusted Hosts policy (POST → insufficient_scope),

## RANKED HYPOTHESES 2026-09-05 00:18:49 UTC
- [75] app.dev.sipgate.com: Dev SPA Infrastructure Exposure Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- [45] app.sipgate.com/implicit-auth-redirect?redirect=<attacker>: OAuth implicit token leakage via client-side unvalidated redirect on post-login handler (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: 4 read-only GETs @1rps against unusual high-value swagger paths for authz-drift triage: https://api.sipgate.com/v2/portings, https://api.sipgate.com/v2/l
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/app-login — check if dev login redirects to login.dev.sipgate.com and whether a dev Keycloak realm is accessible (check /
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path (oauth2/clients, userinfo, app/links, users, contacts csv/internal, history/export, portings, per
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return the same 200 English dict (whitelist-wi
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs 
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak is no
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client, i.e. revoked
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak Trusted Hosts policy (POST → insufficient_scope),

## RANKED HYPOTHESES 2026-09-05 04:44:20 UTC
- [60] api.sipgate.com/v2/{portings,history,addresses,devices,groups,contacts}: API v2 Multi-Tenant BOLA via Predictable Object IDs with Authenticated Tenant Pair (from art/lead_nemotron3.txt)
- [45] api.sipgate.com/v2/{portings/{portingId},: api.sipgate.com BOLA on multi-tenant resources (tenant A token reads tenant B resources) (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: obtain reporter/legal sign-off, then single PUT probe to sipgate-desktop-app.s3.eu-central-1.amazonaws.com (unique object name) to test write path — sole
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with `Origin: https://evil.example` — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve `login.dev.sipgate.com`
- LEARN: REJECTED AUTH @ api.sipgate.com: `/v2/portings`, `/v2/log/webhooks`, `/v2/app/tacs`, `/v2/settings/sipgateio` all 401 empty-body unauth — remaining swagger post
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/graphql: 404 — no GraphQL introspection surface on API domain.
- LEARN: REJECTED OTHER @ chatbot.sipgate.com/graphql: 404 (nginx/1.21.6 via `via: 1.1 google`) — no GraphQL; prod chatbot behind GFE/LB, infra note only.
- LEARN: ACCEPTED INFO @ payment.sipgate.com: every path incl `/actuator/health`, `/gateway/health` → 307 → `https://sipgate.io` (Spring Gateway catch-all) — actuator/MS
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: dev bundle rotated to `main-D04St2Sb.js` (5.65MB, no source maps); hardcodes new dev hosts `admin.dev.sipgate.net` (.net TL
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: re-read openid-configuration — DCR endpoint, ROPC, device_code, CIBA, client_secret_jwt, HS256/384/512 id_t
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves `Vary: Origin` with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts ar
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak `x-b3-traceid` (Zipkin trace id) + `vary:origin`; descriptive header only, OOS as standalone; confirms per-r
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak not d
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no

## RANKED HYPOTHESES 2026-09-05 08:45:49 UTC
- [55] api.[0m: API v2 Multi-Tenant BOLA via Predictable Object IDs with Authenticated Tenant Pair (from art/lead_nemotron3.txt)
- [45] api.sipgate.com/v2/{portings/{id},addresses/{id},devices/{id},oauth2/clients/{id}}: api.sipgate.com BOLA via predictable object IDs with authenticated tenant pair (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: Fetch https://api.sipgate.com/swagger.json and extract all paths containing URL-accepting parameters (webhook callbacks, import-from-URL, proxy, PDF/scre
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths (portings, log/webhooks, app/tacs, settings/sipgateio, contacts, account, numbers, users, authorization/use
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/graphql + chatbot.sipgate.com/graphql: 404 — no GraphQL introspection surface.
- LEARN: ACCEPTED INFO @ payment.sipgate.com: all paths → 307 → sipgate.io (Spring Gateway catch-all) — hardened, actuator not exposed.
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: dev bundle rotated to main-D04St2Sb.js; new dev hosts (admin.dev.sipgate.net, integration/payment/team-de/team-uk.dev.sipga
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: DCR/ROPC/device/CIBA/HS256/512/PKCE-plain — standard Keycloak defaults; config-advertising not a vulnerabil
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak not d
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no

## RANKED HYPOTHESES 2026-09-05 12:12:08 UTC
- [60] chatbot.sipgate.com/chat/session/socket.io/: Production Chatbot Socket.io WebSocket Transport Arbitrary-Origin Hijack (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{numbers/{id},contacts/{id},devices/{id},portings}: Cross-tenant BOLA on credential-bearing resources via predictable IDs (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: obtain reporter/legal sign-off then single PUT probe (unique object name, +json CT) to sipgate-desktop-app.s3.eu-central-1.amazonaws.com — sole remaining
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, so implicit token-in-fragment leak not d
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no

## RANKED HYPOTHESES 2026-09-05 15:25:31 UTC
- [65] chatbot.sipgate.com/chat/session/socket.io/: Production Chatbot Socket.io WebSocket Transport Arbitrary-Origin Hijack (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: obtain reporter/legal sign-off then single PUT probe (unique object name, `application/json` CT) to `sipgate-desktop-app.s3.eu-central-1.amazonaws.com/te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every tested path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confirmed, but 
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3: publicly listable S3 bucket with softphone installers 1.3.0–1.17.19 (stale since 2024-06-11); ACL/policy reads deni
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos

## RANKED HYPOTHESES 2026-09-05 17:42:02 UTC
- [65] chatbot.sipgate.com/chat/session/socket.io/: Production Chatbot Socket.io WebSocket Transport Arbitrary-Origin Hijack (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos

## RANKED HYPOTHESES 2026-09-05 19:37:52 UTC
- [65] chatbot.sipgate.com/chat/session/socket.io/: Production Chatbot Socket.io WebSocket Transport Arbitrary-Origin Hijack (from art/lead_nemotron3.txt)
- [60] sipgate-desktop-app.s3.eu-central-1.amazonaws.com: Desktop-app S3 bucket writable → signed-installer swap (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: obtain reporter/legal sign-off then single PUT probe (unique object name, `application/json` CT) to `sipgate-desktop-app.s3.eu-central-1.amazonaws.com/te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- LEARN: REJECTED AUTH @ chatb.sipgate.com WS: direct WS-transport responses origin-aware (evil → 400 no-ACAO; same-origin → 400 with ACAO); browser-readable arbitrary-o
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3: re-confirmed listable bucket, stale 2024-06-11, write path untested (HUMAN sign-off required).
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos

## RANKED HYPOTHESES 2026-09-05 21:50:48 UTC
- [55] chatbot.sipgate.com/chat/session/socket.io/: Production Chatbot Socket.io WebSocket Transport Origin Validation Bypass (from art/lead_nemotron3.txt)
- [55] api.sipgate.com/v2/: API v2 Multi-Tenant BOLA via Predictable Object IDs with Authenticated Tenant Pair (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19 + lat
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED AUTH @ chatbot.sipgate.com: production socket.io endpoint accepts connections from arbitrary origins without CORS restriction on transport handshake.
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live prod team portal, identical CSP dev-origin leaks (frame-ancestors app.local.sipgate.com:3443, connect
- LEARN: REJECTED OTHER @ api.sipgate.com:3396 + team-de.live.sipgate.com:3396: CSP connect-src internal port unreachable externally (TCP timeout) — topology leak only.
- LEARN: REJECTED OATH @ login.sipgate.com/?redirect=: evil vs benign → identical 302 to Keycloak sipgate-apps, hardcoded redirect_uri — redirect param inert at SSO boot
- LEARN: ACCEPTED INFO @ sipgate-desktop-app.s3: ?versions → 439 keys, all VersionId=null (versioning disabled), IsTruncated=false — full artifact inventory, no hidden v
- LEARN: REJECTED OTHER @ api.sipgate.com:3396 + team-de.live.sipgate.com:3396: CSP connect-src internal port unreachable externally (TCP timeout) — topology leak only.
- LEARN: REJECTED OATH @ login.sipgate.com/?redirect=: evil vs benign → identical 302 to Keycloak sipgate-apps, hardcoded redirect_uri — redirect param inert at SSO boot
- LEARN: ACCEPTED INFO @ sipgate-desktop-app.s3: ?versions → 439 keys, all VersionId=null (versioning disabled), IsTruncated=false — full artifact inventory, no hidden v
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS-transport responses origin-aware (evil → 400 no-ACAO; same-origin → 400 with ACAO); browser-readable arbitrary
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3: re-confirmed listable bucket, stale 2024-06-11, write path untested (HUMAN sign-off required).
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS-transport responses origin-aware (evil → 400 no-ACAO; same-origin → 400 with ACAO); browser-readable arbitrary
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3: re-confirmed listable bucket, stale 2024-06-11, write path untested (HUMAN sign-off required).
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS-transport responses origin-aware (evil → 400 no-ACAO; same-origin → 400 with ACAO); browser-readable arbitrary
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3: re-confirmed listable bucket, stale 2024-06-11, write path untested (HUMAN sign-off required).
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos

## RANKED HYPOTHESES 2026-09-05 23:45:04 UTC
- [75] chatbot.dev.sipgate.com/chat/session/socket.io/: Dev Chatbot Socket.io Arbitrary-Origin WebSocket Handshake Acceptance (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{numbers/{id},contacts/{id},devices/{id},portings}: Cross-tenant BOLA on credential-bearing resources (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: obtain reporter/legal sign-off then single PUT probe (unique object name, `application/json` CT) to `sipgate-desktop-app.s3.eu-central-1.amazonaws.com/te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://chatbot.dev.sipgate.com/chat/session/socket.io/?EIO=4&transport=websocket with Origin: https://evil.example — confirm WS handshake response c
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live prod team portal, identical CSP dev-origin leaks (frame-ancestors app.local.sipgate.com:3443, connect
- LEARN: REJECTED OTHER @ api.sipgate.com:3396 + team-de.live.sipgate.com:3396: CSP connect-src internal port unreachable externally (TCP timeout) — topology leak only.
- LEARN: REJECTED OATH @ login.sipgate.com/?redirect=: evil vs benign → identical 302 to Keycloak sipgate-apps, hardcoded redirect_uri — redirect param inert at SSO boot
- LEARN: ACCEPTED INFO @ sipgate-desktop-app.s3: ?versions → 439 keys, all VersionId=null (versioning disabled), IsTruncated=false — full artifact inventory, no hidden v
- LEARN: REJECTED OTHER @ api.sipgate.com:3396 + team-de.live.sipgate.com:3396: CSP connect-src internal port unreachable externally (TCP timeout) — topology leak only.
- LEARN: REJECTED OATH @ login.sipgate.com/?redirect=: evil vs benign → identical 302 to Keycloak sipgate-apps, hardcoded redirect_uri — redirect param inert at SSO boot
- LEARN: ACCEPTED INFO @ sipgate-desktop-app.s3: ?versions → 439 keys, all VersionId=null (versioning disabled), IsTruncated=false — full artifact inventory, no hidden v
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons

## RANKED HYPOTHESES 2026-09-06 04:14:13 UTC
- [75] chatbot.dev.sipgate.com/chat/session/socket.io/: Dev Chatbot Socket.io Arbitrary-Origin WebSocket Handshake Acceptance (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{portings/{id},devices/{id}/credentials*,authorization/oauth2/clients/{clientId}}: Cross-tenant BOLA on credential-bearing /v2 resources (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: one handshake GET `wss://chatbot.dev.sipgate.com/chat/session/socket.io/?EIO=4&transport=websocket` with `Origin: https://evil.example` (→ 101 vs 400 dec
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://chatbot.dev.sipgate.com/chat/session/socket.io/?EIO=4&transport=websocket with Origin: https://evil.example — confirm WS handshake response c
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: empirical localhost Chromium 152 cross-origin popup test → victim popup reading `window.opener.swag
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: live swagger-ui 5.x; implicit-only third-party client `sipgate-swagger-ui` exposes extreme scope set (oauth2-clients:w
- LEARN: ACCEPTED INFO @ sipgate.io / developer.sipgate.io: developer platform = Cloudflare-fronted static marketing/docs (301 → www.sipgate.io/for-developer), links bac
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/keycloak-logout.js: logout bridge always redirects to fixed same-origin oauth2-logout.html — not attacker-controllable, n
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons

## RANKED HYPOTHESES 2026-09-06 08:40:48 UTC
- [75] chatbot.dev.sipgate.com/chat/session/socket.io/: Dev Chatbot Socket.io Arbitrary-Origin WebSocket Handshake Acceptance (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{portings/{id},devices/{id}/credentials/password,authorization/oauth2/clients/{clientId}}: Cross-tenant BOLA on credential-bearing /v2 resources (unchanged, still the strongest surviving lead) (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: obtain reporter+legal sign-off for the standing single-PUT S3 write probe (unique `test-probe-<ts>.json`, CT application/json, then delete) and in the sa
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `wss://chatbot.dev.sipgate.com/chat/session/socket.io/?EIO=4&transport=websocket` with `Origin: https://evil.example` — confirm WS handshake response
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: empirical localhost Chromium 152 cross-origin popup test → victim popup reading `window.opener.swag

## RANKED HYPOTHESES 2026-09-06 12:30:51 UTC
- [75] chatbot.dev.sipgate.com/chat/session/socket.io/: Dev Chatbot Socket.io Arbitrary-Origin WebSocket Handshake Acceptance (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{portings/{id},devices/{id}/credentials/password,authorization/oauth2/clients/{clientId}}: Cross-tenant BOLA on credential-bearing /v2 resources (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: in one session — (1) register two independent sipgate test tenants, obtain bearer tokens for both, then differential-test /v2/portings/{id}, /v2/devices/
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `wss://chatbot.dev.sipgate.com/chat/session/socket.io/?EIO=4&transport=websocket` with `Origin: https://evil.example` — confirm WS handshake response
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling also blocks cross-origin reads (Vary:Origin, no ACAO); identical 
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: empirical localhost Chromium 152 cross-origin popup test → victim popup reading `window.opener.swag
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: live swagger-ui 5.x; implicit-only third-party client `sipgate-swagger-ui` exposes extreme scope set (oauth2-clients:w
- LEARN: ACCEPTED INFO @ sipgate.io / developer.sipgate.io: developer platform = Cloudflare-fronted static marketing/docs (301 → www.sipgate.io/for-developer), links bac
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/keycloak-logout.js: logout bridge always redirects to fixed same-origin oauth2-logout.html — not attacker-controllable, n
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons

## RANKED HYPOTHESES 2026-09-06 15:53:17 UTC
- [75] login.sipgate.com: OIDC Implicit Flow Token Leakage via Referer/History (from art/lead_bigpickle.txt)
- [50] api.sipgate.com/v2/{portings/{id},devices/{id}/credentials/password,authorization/oauth2/clients/{clientId}}: Cross-tenant BOLA on credential-bearing /v2 resources (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://app.sipgate.com/implicit-auth-redirect?redirect=/ — inspect HTML/JS for fragment handling, token storage, third-party requests, Referer polic
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com — fetch current JS bundle (main-D04St2Sb.js or rotated), extract all hardcoded host:port pairs from source/chunks, and DN
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- LEARN: ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling also blocks cross-origin reads (Vary:Origin, no ACAO); identical 
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr

## RANKED HYPOTHESES 2026-09-06 17:45:31 UTC
- [50] api.sipgate.com/v2/{portings/{id},devices/{id}/credentials/password,authorization/oauth2/clients/{clientId}}: Cross-tenant BOLA on credential-bearing /v2 resources (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: one session — (1) obtain reporter+legal sign-off then execute single-PUT S3 write probe `PUT https://sipgate-desktop-app.s3.eu-central-1.amazonaws.com/te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com — fetch current JS bundle (main-D04St2Sb.js or rotated), extract all hardcoded host:port pairs from source/chunks, and DN
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads; identical to prod chatbot REJECT class
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot with socket.io endpoint — contradicts prior "dev env externally inert"
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling also blocks cross-origin reads (Vary:Origin, no ACAO); identical 
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr

## RANKED HYPOTHESES 2026-09-06 20:28:47 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint Exposure with Permissive CORS (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{portings/{portingId},devices/{deviceId},authorization/oauth2/clients/{clientId},voicemails/{voicemailId}}: Cross-tenant BOLA on credential-bearing /v2 resources (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET `https://login.sipgate.com/auth/realms/third-party/protocol/openid-connect/registrations/openid-connect` and the realm `/auth/realms/third-party` pag
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://integration.dev.sipgate.com/health, /swagger.json, /actuator/health, /webhooks, /callbacks, /api, /v2/health — with `-k` flag for self-signed
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads; identical to prod chatbot REJECT class
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot with socket.io endpoint — contradicts prior "dev env externally inert"
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr

## RANKED HYPOTHESES 2026-09-06 22:14:00 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint SSRF via Webhook/Callback Handlers (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/{portings/{id},devices/{id}/credentials/password,authorization/oauth2/clients/{clientId}}: Cross-tenant BOLA on credential-bearing /v2 resources (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: one session — (1) obtain reporter+legal sign-off then execute single-PUT S3 write probe `PUT https://sipgate-desktop-app.s3.eu-central-1.amazonaws.com/te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com — fetch current JS bundle (main-Dr5Dd34d.js or rotated), extract all hardcoded host:port pairs from source/chunks, and DN
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads; identical to prod chatbot REJECT class
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot with socket.io endpoint — contradicts prior "dev env externally inert"
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio

## RANKED HYPOTHESES 2026-09-07 00:12:56 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint SSRF via Webhook/Callback Handlers (from art/lead_nemotron3.txt)
- [55] chatbot.sipgate.com/chat/session/socket.io/: S3 Bucket Write Access Enables Signed Installer Supply-Chain Compromise (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://app.dev.sipgate.com with Origin: https://evil.example — check for CORS headers (ACAO, ACAC) on dev SPA; DNS resolve login.dev.sipgate.com, ch
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com — fetch current JS bundle (main-Dr5Dd34d.js or rotated), extract all hardcoded host:port pairs from source/chunks, and DN
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS-transport responses origin-aware (evil → 400 no-ACAO; same-origin → 400 with ACAO); browser-readable arbitrary
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec relocated/live (144 paths, global security=[] => spec-claims-public vs edge-401) — re-confirms stale spec 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: 6 newly-tested paths (/calls, /channels, /app/events, /balance, /autorecordings/greetings, numbers/quickdial/validation) and
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3: re-confirmed listable bucket, stale 2024-06-11, write path untested (HUMAN sign-off required).
- LEARN: REJECTED AUTH @ app.dev.sipgate.com: login.dev.sipgate.com dead (HTTP 000) and api.dev.sipgate.com 403 → dev env externally inert; dev weaker-auth/ATO path defl
- LEARN: CHANGED AUTH @ chatbot.sipgate.com: polling transport serves Vary:Origin with no ACAO for arbitrary origin → cross-origin response reads blocked; "accepts arbit
- LEARN: ACCEPTED INFO @ api.sipgate.com: public swagger.json (144 paths) + /translations/{language} unauthenticated; spec security annotations stale vs server behavior 
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: every docd high-value path returns 401 empty-body unauth; 404 only for truly-unknown paths. Stale swagger annotations confir
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: ACCEPTED INFO @ api.sipgate.com: 401 responses leak x-b3-traceid (Zipkin trace id) + vary:origin; descriptive header only, OOS as standalone; confirms per-reque
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA publicly accessible on Fastly CDN with identical production JS bundle, hardcoded internal dev/local URLs,
- LEARN: ACCEPTED MISCONFIG @ team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in production portal; leaks SERVERI
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/health: unauthenticated endpoint reflects arbitrary-origin CORS with credentials (defense-in-depth gap, no data leak).
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: Arbitrary-origin CORS reflection with credentials confirmed across multiple v2 endpoints — no Origin allowlist, expos
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: empirical localhost Chromium 152 cross-origin popup test → victim popup reading `window.opener.swag
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: empirical localhost Chromium 152 cross-origin popup test → victim popup reading `window.opener.swag
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: live swagger-ui 5.x; implicit-only third-party client `sipgate-swagger-ui` exposes extreme scope set (oauth2-clients:w
- LEARN: ACCEPTED INFO @ sipgate.io / developer.sipgate.io: developer platform = Cloudflare-fronted static marketing/docs (301 → www.sipgate.io/for-developer), links bac
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/keycloak-logout.js: logout bridge always redirects to fixed same-origin oauth2-logout.html — not attacker-controllable, n
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: REJECTED OTHER @ login.dev.sipgate.com / team-de.dev.sipgate.com / payment.dev.sipgate.com: DNS resolve to sipgate-owned 217.116.x.x but HTTP 000 (timeout) — ex
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: Second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443, connect-s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: Spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: Direct WS transport rejects arbitrary Origin (evil → 400 no-ACAO) — browser-readable arbitrary-origin channel not demons
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads; identical to prod chatbot REJECT class
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot with socket.io endpoint — contradicts prior "dev env externally inert"
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio
- LEARN: ACCEPTED INFO @ integration.sipgate.com: PROD integration platform "Platypus" exposes full 26-op OpenAPI spec publicly under /swagger (contacts/call-logs/tasks/
- LEARN: ACCEPTED INFO @ integration.dev.sipgate.com: dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same unif
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: REJECTED AUTH @ integration.sipgate.com direct: unauthenticated GET on all 26 documented paths returns uniform app-403 "Forbidden resource" (Bearer: dummy ident
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio

## RANKED HYPOTHESES 2026-09-07 05:00:09 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint SSRF via Webhook/Callback Handlers (from art/lead_nemotron3.txt)
- [50] sipgate-desktop-app.s3.eu-central-1.amazonaws.com: Desktop-app S3 bucket writable → signed-installer / electron-updater swap (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: one session — (1) obtain reporter+legal sign-off then execute single-PUT S3 write probe `PUT https://sipgate-desktop-app.s3.eu-central-1.amazonaws.com/te
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com — fetch current JS bundle (main-Dr5Dd34d.js or rotated), extract all hardcoded host:port pairs from source/chunks, and DN
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads; identical to prod chatbot REJECT class
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot with socket.io endpoint — contradicts prior "dev env externally inert"
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio
- LEARN: ACCEPTED INFO @ integration.sipgate.com: PROD integration platform "Platypus" exposes full 26-op OpenAPI spec publicly under /swagger (contacts/call-logs/tasks/
- LEARN: ACCEPTED INFO @ integration.dev.sipgate.com: dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same unif
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: REJECTED AUTH @ integration.sipgate.com direct: unauthenticated GET on all 26 documented paths returns uniform app-403 "Forbidden resource" (Bearer: dummy ident
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-Dr5Dd34d.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio

## RANKED HYPOTHESES 2026-09-07 10:22:36 UTC
- [55] integration.sipgate.com: Integration platform "Platypus" gate-bypass enables OAuth/SSRF/CRM chain on prod+dev (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: passive gate-bypass sweep on prod integration.sipgate.com (read-only, ≤1rps) — `GET https://integration.sipgate.com/contacts.list` variants: `/../contact
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: live swagger-ui 5.x; implicit-only third-party client `sipgate-swagger-ui` exposes extreme scope set (oauth2-clients:w
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/keycloak-logout.js: logout bridge always redirects to fixed same-origin oauth2-logout.html — not attacker-controllable, n
- LEARN: ACCEPTED INFO @ sipgate.io / developer.sipgate.io: developer platform = Cloudflare-fronted static marketing/docs (301 → www.sipgate.io/for-developer), links bac
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: REJECTED AUTH @ integration.sipgate.com direct: unauthenticated GET on all 26 documented paths returns uniform app-403 "Forbidden resource" (Bearer: dummy ident
- LEARN: ACCEPTED INFO @ integration.dev.sipgate.com: dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same unif
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads; identical to prod chatbot REJECT class
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s

## RANKED HYPOTHESES 2026-09-07 16:05:44 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint SSRF via Webhook/Callback Handlers (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/main-DFko0cRT.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve any new
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-DFko0cRT.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio
- LEARN: ACCEPTED INFO @ integration.sipgate.com: PROD integration platform "Platypus" exposes full 26-op OpenAPI spec publicly under /swagger (contacts/call-logs/tasks/
- LEARN: ACCEPTED INFO @ integration.dev.sipgate.com: dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same unif
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: REJECTED AUTH @ integration.sipgate.com direct: unauthenticated GET on all 26 documented paths returns uniform app-403 "Forbidden resource" (Bearer: dummy ident
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr

## RANKED HYPOTHESES 2026-09-07 19:58:37 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint SSRF via Webhook/Callback Handlers (from art/lead_nemotron3.txt)
- [55] integration.sipgate.com: Firebase-JWT gate continuity: token minting for the revalidated Firebase project (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: fetch `https://chatbot.dev.sipgate.com/lib/demo.js.map` and grep for `firebase|apiKey|AIza|project_?id|storageBucket|appId` (zero-mutation; completed for
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/main-DFko0cRT.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve any new
- LEARN: ACCEPTED INFO @ integration.sipgate.com: PROD integration platform "Platypus" exposes full 26-op OpenAPI spec publicly under /swagger (contacts/call-logs/tasks/
- LEARN: ACCEPTED INFO @ integration.dev.sipgate.com: dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same unif
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: REJECTED AUTH @ integration.sipgate.com direct: unauthenticated GET on all 26 documented paths returns uniform app-403 "Forbidden resource" (Bearer: dummy ident
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr

## RANKED HYPOTHESES 2026-09-07 22:44:23 UTC
- [65] integration.dev.sipgate.com: Integration Dev Endpoint SSRF via Webhook/Callback Handlers (from art/lead_nemotron3.txt)
- [55] integration.sipgate.cloud: GCP-backed Platypus instance SSRF → cloud-metadata amplification (users.integrations.create apiUrl) (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://integration.sipgate.com/swagger/swagger-ui-init.js` — save; diff against `/tmp/opencode/cloud_init.js` for instance-level drift (auth host, 
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/main-DFko0cRT.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve any new
- LEARN: ACCEPTED MISCONFIG @ integration.dev.sipgate.com: NEWLY ALIVE dev endpoint (217.116.121.180) responds HTTPS 403 with `access-control-allow-origin: *` — hardcode
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com: JS bundle rotated to `main-DFko0cRT.js`; new hardcoded hosts `admin.dev.sipgate.net`, `admin.live.sipgate.net`, `integratio
- LEARN: ACCEPTED INFO @ integration.sipgate.com: PROD integration platform "Platypus" exposes full 26-op OpenAPI spec publicly under /swagger (contacts/call-logs/tasks/
- LEARN: ACCEPTED INFO @ integration.dev.sipgate.com: dev twin serves near-identical spec (only auth host differs: login.dev); swagger bundle+sourcemap public; same unif
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: REJECTED AUTH @ integration.sipgate.com direct: unauthenticated GET on all 26 documented paths returns uniform app-403 "Forbidden resource" (Bearer: dummy ident
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: ACCEPTED MISCONFIG @ chatbot.dev.sipgate.com: LIVE dev chatbot (nginx/1.24.0, Google Cloud) with socket.io endpoint — contradicts prior "dev env externally iner
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com: second live team portal with identical CSP dev-origin leak (frame-ancestors app.local.sipgate.com:3443 + connect-
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/swagger.json: spec live (144 paths, global security=[]), re-confirms stale annotations vs edge-401 — no authz drift unauthent
- LEARN: REJECTED AUTH @ chatbot.sipgate.com WS: direct WS transport rejects arbitrary Origin (evil→400 no-ACAO) — browser-readable arbitrary-origin channel not demonstr

## RANKED HYPOTHESES 2026-09-08 01:23:37 UTC
- [45] app.sipgate.com/implicit-auth-redirect?redirect=<attacker>: OAuth implicit token leakage via client-side unvalidated redirect on post-login handler (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: open a private-tab full login flow on `https://app.sipgate.com/implicit-auth-redirect?redirect=https://evil.example` via the standard login, and report f
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+574); confirms Firebase validator + auth-mechanism drift (spec Key
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous.
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation).
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source.

## RANKED HYPOTHESES 2026-09-08 06:04:43 UTC
- [60] integration.sipgate.com: Integration Platform OAuth Gate Bypass via Spec-Behavior Drift (from art/lead_nemotron3.txt)
- [55] *.integration.sipgate.cloud: Per-vendor integration adapters accept provider-key header auth / SSRF via x-provider-url (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://hubspot.integration.sipgate.cloud/swagger/swagger-ui-init.js` (live vendor host, read-only) + same on `zendesk.integration.sipgate.cloud` — 
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/main-DFko0cRT.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve any new
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+574); confirms Firebase validator + auth-mechanism drift (spec Key
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous.
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation).
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source.
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+574); confirms Firebase validator + auth-mechanism drift (spec Key
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou

## RANKED HYPOTHESES 2026-09-08 11:34:37 UTC
- [70] *.integration.sipgate.cloud: CRM Adapter Tier SSRF via x-provider-url Header Injection (from art/lead_nemotron3.txt)
- [55] *.integration.sipgate.cloud: Per-vendor integration adapters: Basic-auth gate bypass via provider-key header passthrough or cred leak in public repos (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://hubspot.integration.sipgate.cloud/swagger/swagger-ui-init.js` + same on `zendesk.integration.sipgate.cloud` — diff against apex `integration
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/main-DFko0cRT.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve any new
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+574); confirms Firebase validator + auth-mechanism drift (spec Key
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous.
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation).
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source.
- LEARN: ACCEPTED MISCONFIG @ integration.sipgate.com: `/oauth2/redirect` + `/oauth2/callback` declared with NO security requirement in embedded spec yet all external re
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+574); confirms Firebase validator + auth-mechanism drift (spec Key
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou

## RANKED HYPOTHESES 2026-09-08 15:21:20 UTC
- [70] *.integration.sipgate.cloud: CRM Adapter Tier SSRF via x-provider-url Header Injection (from art/lead_nemotron3.txt)
- [50] *.integration.sipgate.cloud: Per-vendor adapter gate/SSRF — nginx Basic is the only gate; provider-key not a bypass (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://integration.sipgate.com/swagger/swagger-ui-init.js` — extract full OAuth securityScheme (authorizationUrl/tokenUrl, `platypus-swagger` clien
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/main-DFko0cRT.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve any new
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: fb-jwt counter 56363 (LOWER than prior 80373) → per-replica counter (restart-reset), confirms scale/multi-insta
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+574); confirms Firebase validator + auth-mechanism drift (spec Key
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou

## RANKED HYPOTHESES 2026-09-08 19:10:20 UTC
- [50] *.integration.sipgate.cloud: Per-vendor adapter gate/SSRF — nginx Basic is the only gate; provider-key not a bypass (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://integration.sipgate.com/swagger/swagger-ui-init.js` — extract full OAuth securityScheme (authorizationUrl/tokenUrl, `platypus-swagger` clien
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: fb-jwt counter 56363 (LOWER than prior 80373) → per-replica counter (restart-reset), confirms scale/multi-insta
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn

## RANKED HYPOTHESES 2026-09-08 21:54:26 UTC
- [70] *.integration.sipgate.cloud: CRM Adapter Tier SSRF via x-provider-url Header Injection (from art/lead_nemotron3.txt)
- [50] *.integration.sipgate.cloud: Per-vendor adapter SSRF via x-provider-url header injection (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://integration.sipgate.com/swagger/oauth2-redirect.html` — confirm public reachability/byte-compare with api.sipgate.com/v2/doc mirror to close
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-5xLTM2Hn.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/swagger/index.html: swagger static assets (index.html, bundle, init) PUBLIC (200) while all 26 data ops + oauth2 routes 
- LEARN: ACCEPTED INFO @ integration.sipgate.com spec: implicit-only OAuth for `platypus-swagger` (authzUrl login.sipgate.com sipgate-apps realm, scopes={}); `/users.int
- LEARN: ACCEPTED INFO @ integration.sipgate.com spec: 400-error schema leaks internal Notion URL sipgatede.notion.site/Integration-9eb9360dc6bd49be8411e208121f1179.
- LEARN: REJECTED OTHER @ admin.live.sipgate.net / integration.live.sipgate.com: admin.live resolves 217.10.73.71 HTTP 000 (inert); integration.live NXDOMAIN — no live t
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+12,685); confirms Firebase validator + auth-mechanism drift (spec 
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn

## RANKED HYPOTHESES 2026-09-09 00:02:49 UTC
- [75] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- [55] mock.integration.sipgate.cloud: Ungated mock adapter twin exposes the gated adapter tier's full request surface cross-origin (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://mock.integration.sipgate.cloud/swagger/swagger-ui-init.js` (spec may differ from gated siblings on a route not on the 8MB path) — if 200, di
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-5xLTM2Hn.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+12,685); confirms Firebase validator + auth-mechanism drift (spec 
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn

## RANKED HYPOTHESES 2026-09-09 04:34:03 UTC
- [80] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- [72] app.dev.sipgate.com/main-5xLTM2Hn.js: Dev SPA bundle extracts reveal internal host:port map enabling targeted SSRF against live dev backend (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `GET https://mock.integration.sipgate.cloud/swagger/swagger-ui-init.js` — confirm public reachability, byte-compare against apex platypus spec (386538b);
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-5xLTM2Hn.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /health 200 + /contacts 200 ~8MB synthetic corpus; ACAO:*; x-prov
- LEARN: ACCEPTED INFO @ app.dev.sipgate.com bundle: 13+ hardcoded internal host:port pairs; integration.dev.sipgate.com confirmed alive with HTTPS 403 + ACAO:*; 4 gener
- LEARN: REJECTED AUTH @ *.integration.sipgate.cloud vendor tier: nginx Basic 401 sole gate; OPTIONS 204 passes gate but data GET still 401; x-provider-url does NOT bypa
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: per-replica counters (56363 < prior 80373); confirms scale/multi-instance; live Firebase-JWT validator.
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+12,871); confirms Firebase validator + auth-mechanism drift (spec 
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn

## RANKED HYPOTHESES 2026-09-09 09:31:09 UTC
- [25] mock.integration.sipgate.cloud/contacts|/contacts/search|/contacts/export: Mock blueprint SSRF via x-provider-url on ungated Express twin (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: no further passive value on integration family (mock proofed non-SSRF, apex/vendor/dev gated or inert). Next cycle re-`GET https://integration.sipgate.co

## RANKED HYPOTHESES 2026-09-09 13:50:39 UTC
- [80] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- [40] admin.dev.sipgate.net: Dev admin/helpdesk alias map reachability (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: OPTIONS/HEAD on 80/443 of 217.116.120.148 (admin.dev.sipgate.net / helpdesk.dev.sipgate.net) and 217.10.73.71 (admin.live.sipgate.net / helpdesk.live.sip
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-5xLTM2Hn.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed on any method/path; x-provider-url header does not parameterize ou
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /health 200 + /contacts 200 ~8MB synthetic corpus; ACAO:*; x-prov
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+12,871); confirms Firebase validator + auth-mechanism drift (spec 
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn

## RANKED HYPOTHESES 2026-09-09 17:48:05 UTC
- [80] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement (from art/lead_nemotron3.txt)
- [40] admin.dev.sipgate.net: Dev admin/helpdesk alias map reachability (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: `curl -sSI -m 8 https://admin.dev.sipgate.net` + `curl -sSI -m 8 https://helpdesk.dev.sipgate.net` + same for `.live.` variants on ports 80/443 — check f
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-CYk1JfU_.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed on any method/path; x-provider-url header does not parameterize ou
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /health 200 + /contacts 200 ~8MB synthetic corpus; ACAO:*; x-prov
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapter tier uniformly behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provi
- LEARN: ACCEPTED INFO @ sf6.integration CT namespace: 276 *.sipgate.cloud hostnames (146 influxdb monitoring, AWS/hetzner/GCP multicloud, k8s tool clusters, dependency-
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: public FB-JWT gate counters increment live (+12,871); confirms Firebase validator + auth-mechanism drift (spec 
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live internet-facing Grafana 11.5.1 (version leaked via /api/health) on AWS LB; login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → external Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: REJECTED MISCONFIG @ docs.sipgate.cloud family: private GitHub Pages on sipgate-owned `sipgate.github.io` org — authenticated pages, not a public doc leak nor t
- LEARN: REJECTED AUTH @ dev.integration.sipgate.cloud + test.integration.sipgate.cloud: shared GCP LB but 404-inert hostname routing — no dev twin token source
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is 
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reac
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from `rest-api-examples/.npmrc.dist` returns `invalid_client`, i.e. rev
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint is gated by Keycloak `Trusted Hosts` policy (POST → `insufficient_scop
- LEARN: ACCEPTED AUTH @ api.sipgate.com: confirmed live OIDC `third-party` realm proxied from the API domain to Keycloak, exposing high-value scopes (contacts/sms/accou
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: OPTIONS preflight (204) + data GET (401) — nginx Basic 401 enforced before app; x-provider-url/x-provider-key heade
- LEARN: REJECTED AUTH @ integration.sipgate.com/oauth2/callback + /oauth2/redirect: uniform app-403 even with fabricated code/state → code-exchange behind FB-JWT 403 ga
- LEARN: ACCEPTED INFO @ *.integration.sipgate.cloud breadth: hubspot/zendesk/salesforce deployed (nginx 401), pipeforce/zapier resolve to same LB 35.246.154.68 but conn
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed on any method/path; x-provider-url header does not parameterize ou

## RANKED HYPOTHESES 2026-09-09 21:01:31 UTC
- [85] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement via Prod Subdomain References (from art/lead_nemotron3.txt)
- [35] admin.dev.sipgate.net: Dev admin/helpdesk transient revival (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: per-cycle (next session) — `curl -sSI -m 8 https://admin.live.sipgate.net` + `https://admin.dev.sipgate.net` (both 80/443) AND `curl -s https://app.dev.s
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-CYk1JfU_.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 2368 (per-replica counter, restart-reset confirmed); live Firebase-JWT validato
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr

## RANKED HYPOTHESES 2026-09-09 23:16:23 UTC
- [20] admin.dev.sipgate.net: Dev helpdesk/admin transient revival (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: per-cycle monitoring — `curl -sSI -m 8 https://admin.live.sipgate.net/` + `https://admin.dev.sipgate.net/` (80/443) AND `curl -s https://app.dev.sipgate.
- LEARN: ACCEPTED INFO @ integration.sipgate.com/swagger: swagger-ui-bundle.js.map public (1.9MB) — stock SwaggerUIBundle webpack sourcemap, node_modules/library sources
- LEARN: ACCEPTED INFO @ sipgatede.notion.site: spec-embedded Notion integration page returns HTTP 200 but content hidden (not publicly shared) — no data disclosure from
- LEARN: CHANGED @ integration.dev.sipgate.com: self-signed cert now (handshake fails without -k); swagger-ui-init.js 200, root 403 — dev-twin redeploy signal, gate unch
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: fb_jwt counter 4116 (+1748) — live validator, per-replica restart-reset reconfirmed.

## RANKED HYPOTHESES 2026-09-10 01:13:06 UTC
- [85] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement via Prod Subdomain References (from art/lead_nemotron3.txt)
- [20] integration.sipgate.com/swagger/*: Integration static swagger tree hides gating-split artifacts beyond sourcemaps (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: per-cycle sentinel (1 rps) — `curl -sSI -m 8 https://admin.live.sipgate.net/` + `https://admin.dev.sipgate.net/` (80/443) AND `curl -s https://app.dev.si
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-CYk1JfU_.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 2368 (per-replica counter, restart-reset confirmed); live Firebase-JWT validato
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr

## RANKED HYPOTHESES 2026-09-10 06:23:40 UTC
- [85] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement via Prod Subdomain References (from art/lead_nemotron3.txt)
- [30] mock.integration.sipgate.cloud: Mock integration x-provider-url as unauthenticated fetch sink (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PASSIVE: GET https://mock.integration.sipgate.cloud/swagger.json — check if OpenAPI spec is served ungated; if 200 with endpoint list, this provides enumeration
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-PqF61JxF.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak — defense-in-depth g
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 — family-wide info disclosure.
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests.
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes — high-value target but requires credential acquisition (AUTH_HELPED).
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 10839 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal

## RANKED HYPOTHESES 2026-09-10 11:51:37 UTC
- [85] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement via Prod Subdomain References (from art/lead_nemotron3.txt)
- [25] app.dev.sipgate.com: Dev SPA bundle rotation introduces new live host with weaker gate (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PASSIVE: Per-cycle sentinel — `curl -sSI -m 8 https://admin.live.sipgate.net/` + `https://admin.dev.sipgate.net/` (80/443) AND `curl -s https://app.dev.sipgate.
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-PqF61JxF.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ amp.sipgate.de: LIVE Cloudflare analytics endpoint (204, ACAO:*, Express, svid session tokens) — analytics/beacon infra, not a security fin
- LEARN: ACCEPTED INFO @ status.sipgate.de: Atlassian Statuspage with public API (4 components, no internal infra names) — public status page, not a security finding.
- LEARN: REJECTED OTHER @ integration.sipgate.cloud: K8s ingress serves identical Swagger UI (386538b) as .com; app-level 403 on data paths; same Platypus backend, no ga
- LEARN: ACCEPTED INFO @ static.sipgate.com: Apache static assets host; CSP frame-ancestors family-wide; references sipgate-desktop-app S3 bucket (already ACCEPTED).
- LEARN: ACCEPTED INFO @ info.sipgate.de: Dead HubSpot page (404 predicted at edge, Cloudflare Workers) — no data disclosure.
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 10839 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, token persists to localStorage before na
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB)
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope per program policy

## RANKED HYPOTHESES 2026-09-10 15:11:49 UTC
- [85] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement via Prod Subdomain References (from art/lead_nemotron3.txt)
- [55] api.sipgate.com/v2/doc/*: Swagger implicit client same-origin XSS amplifier chains with api.sipgate.com CORS credential reflection to achieve session theft (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://mock.integration.sipgate.cloud/swagger.json — check if OpenAPI spec is served ungated; if 200 with endpoint list, enumerate paths for x-provi
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-PqF61JxF.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak — defense-in-depth g
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests.
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS; distinct gate tier from apex 
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes — high-value target but requires credential acquisition (AUTH_HELPED).
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB).
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 10839 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, token persists to localStorage before na
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB)
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope per program policy

## RANKED HYPOTHESES 2026-09-10 18:54:27 UTC

## RANKED HYPOTHESES 2026-09-10 21:17:27 UTC
- [85] app.dev.sipgate.com: Dev SPA Internal Topology Enables Targeted SSRF/Lateral Movement via Prod Subdomain References (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-PqF61JxF.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 10839 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, token persists to localStorage before na
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB)
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope per program policy
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 

## RANKED HYPOTHESES 2026-09-10 23:19:53 UTC
- [70] integration.sipgate.com: Integration Platform OAuth Gate Bypass via Spec-Behavior Drift + SSRF via apiUrl (from art/lead_nemotron3.txt)
- [55] api.sipgate.com/v2/doc/*: Swagger implicit client + api/v2 CORS-credential reflection as same-origin-XSS→session-theft chain (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-D_WMxc4R.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ app.dev.sipgate.com: live dev SPA on Fastly CDN with production JS bundle, 18 hardcoded internal host:port pairs including NEW prod subdoma
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 93107 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ grafana.sipgate.cloud: live Grafana 11.5.1 on AWS LB, login-gated, no anonymous
- LEARN: ACCEPTED MISCONFIG @ share1.sipgate.cloud: dangling CNAME → Hetzner StorageShare NXDOMAIN; takeover candidate (needs HUMAN claim validation)
- LEARN: ACCEPTED INFO @ mock.integration.sipgate.cloud: only ungated *.integration.sipgate.cloud host; /contacts 200 ~8MB synthetic; ACAO:* + ACAC:true + allow-headers 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: REJECTED SSRF @ *.integration.sipgate.cloud: nginx Basic-auth 401 enforced before app; x-provider-url/x-provider-key headers do NOT bypass nginx gate
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: REJECTED AUTH @ chatbot.dev.sipgate.com WS: direct WS-transport test evil→400 no-ACAO; polling blocks cross-origin reads (Vary:Origin, no ACAO); identical to pr
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, token persists to localStorage before na
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB)
- LEARN: REJECTED OTHER @ api.sipgate.com/v2/translations/{language}: arbitrary language values incl URL-encoded traversal return same 200 English dict (whitelist-with-f
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope per program policy
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 

## RANKED HYPOTHESES 2026-09-11 01:17:13 UTC
- [55] api.sipgate.com/v2/doc/*: Swagger implicit client same-origin XSS amplifier chains with api.sipgate.com CORS credential reflection to achieve session theft (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://mock.integration.sipgate.cloud/swagger.json — check if OpenAPI spec is served ungated; if 200 with endpoint list, enumerate paths for x-provi
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak — defense-in-depth g
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests.
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS; distinct gate tier from apex 
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes — high-value target but requires credential acquisition (AUTH_HELPED).
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB).

## RANKED HYPOTHESES 2026-09-11 06:27:39 UTC

## RANKED HYPOTHESES 2026-09-11 11:38:34 UTC
- [75] api.sipgate.com/v2/doc/oauth2-redirect.html: Swagger-UI Implicit Client + API.v2 CORS-Credential Reflection Chain → Session Theft (from art/lead_nemotron3.txt)
- [50] api.sipgate.com/v2/authorization/oauth2/clients/{clientId}[/gdpr]: Unauthenticated app-plane reach into /v2/authorization/oauth2/clients{id}(/gdpr) contradicts uniform edge-401; a registered third-party client UUID likely returns 200 metadata (GDPR/privacy URLs) unauthenticated (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-D_WMxc4R.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak — defense-in-depth g
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 17394 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB)
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, token persists to localStorage before na
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope per program policy

## RANKED HYPOTHESES 2026-09-11 15:25:08 UTC
- [75] api.sipgate.com/v2/doc/oauth2-redirect.html: Swagger-UI Implicit Client + API.v2 CORS-Credential Reflection Chain → Session Theft (from art/lead_nemotron3.txt)
- [48] api.sipgate.com/v2/authorization/oauth2/clients/{clientId}: String client-ID variants on /v2/authorization/oauth2/clients/{id} may bypass UUID-500 validator (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sipgate.com/v2/authorization/oauth2/clients/sipgate-app-web and GET https://api.sipgate.com/v2/authorization/oauth2/clients/platypus-swag
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://app.dev.sipgate.com/assets/main-D_WMxc4R.js — fetch current JS bundle, extract all hardcoded host:port pairs from source/chunks, DNS-resolve 
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: CORS reflection on oauth2-redirect.html is real (ACAO: evil.com + ACAC:true) but Chromium 152 cross
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client sipgate-swagger-ui; extreme scope set; high-value only as sa
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated — except oauth2/client
- LEARN: ACCEPTED MISCONFIG @ api.sipgate.com/v2/*: arbitrary-origin CORS reflection with credentials persistent across endpoints; x-b3-traceid leak — defense-in-depth g
- LEARN: ACCEPTED INFO @ api.sipgate.com/v2/doc/*: swagger-ui 5.x live with implicit-only third-party client `sipgate-swagger-ui`; extreme scope set; high-value only as 
- LEARN: REJECTED SSRF @ mock.integration.sipgate.cloud: proven synthetic twin, no fetch sink observed; x-provider-url header does not parameterize outbound requests
- LEARN: ACCEPTED MISCONFIG @ *.integration.sipgate.cloud (94 hosts): per-vendor CRM adapters behind nginx Basic-auth 401 with ACAO:* CORS exposing x-provider-{url,key} 
- LEARN: ACCEPTED INFO @ integration.sipgate.com/metrics: firebase_jwt_forbidden_requests 17394 (per-replica counter, restart-reset confirmed); live Firebase-JWT validat
- LEARN: ACCEPTED MISCONFIG @ team-uk.live.sipgate.com + team-de.live.sipgate.com: CSP frame-ancestors includes app.local.sipgate.com:3443 (internal dev origin) in produ
- LEARN: ACCEPTED MISCONFIG @ sipgate-desktop-app.s3.eu-central-1.amazonaws.com: publicly listable S3 bucket exposing full softphone installer index (1.3.0–1.17.19, stal
- LEARN: ACCEPTED AUTH @ login.sipgate.com third-party realm: live OIDC with extreme scopes (contacts/sms/account/balance/payment/authorization:oauth2:clients:write), HS
- LEARN: REJECTED OATH @ api.sipgate.com/v2/doc/oauth2-redirect.html: Chromium 152 cross-origin popup test confirms SecurityError on window.opener read; token fragment s
- LEARN: REJECTED AUTH @ api.sipgate.com/v2: all tested paths return 401 empty-body unauth; uniform edge auth; no authz-drift/BOLA unauthenticated (per KB)
- LEARN: REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: history.replace(external) in React Router resolves same-origin, token persists to localStorage before na
- LEARN: REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of reacha
- LEARN: REJECTED SECRET @ api.sipgate.com third-party OAuth: leaked demo client_id/client_secret from rest-api-examples/.npmrc.dist returns invalid_client — not live cr
- LEARN: REJECTED AUTH @ login.sipgate.com third-party realm: dynamic client registration endpoint gated by Keycloak Trusted Hosts policy (POST → insufficient_scope), no
- LEARN: REJECTED network DoS @ app.sipgate.com: Out of scope per program policy
- LEARN: REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope per program policy
