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
