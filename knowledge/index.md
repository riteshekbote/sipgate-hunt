# Knowledge Base (seed)
- 2026-09-03 REJECTED network DoS @ app.sipgate.com: Out of scope per program policy.
- 2026-09-03 REJECTED SSL/TLS best practice @ login.sipgate.com: Out of scope.
- 2026-09-03 ACCEPTED AUTH @ login.sipgate.com: OIDC implicit flow with fragment token delivery is in-scope high-value target.
- 2026-09-03 REJECTED OATH @ app.sipgate.com/implicit-auth-redirect: `history.replace(external)` in React Router resolves same-origin, so implicit token-in-fragment leak is not demonstrable statically; token persists to localStorage before navigation — fragment never forwarded off-origin. Class signal: unvalidated client redirect is not by itself token theft.
- 2026-09-03 REJECTED AUTH @ login.sipgate.com Keycloak: realm metadata advertising HS256/PKCE-plain/client_secret_jwt is standard Keycloak config, not affirmative of a reachable flawed verifier; treat as config hardening, not a vulnerability.
