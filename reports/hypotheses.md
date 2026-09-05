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
