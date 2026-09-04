## REPOSCAN 2026-09-03 16:31:42 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-03 19:29:27 UTC
class: MISCONFIG
asset: clinq-bridge-sipgate/k8s/template/deployment.yml:23
confidence: 95
reasoning: The K8s deployment template hardcodes `REDIS_URL: rediss://10.37.248.211:6378` — an internal RFC-1918 IP address with a TLS-enabled Redis endpoint. This is in a public GitHub repo under the `sipgate` org, exposing internal infrastructure topology (private network IP, port, TLS config). The `cloudbuild.yaml` in the same repo reveals the GCP project ID (`clinq-services`) and cluster zone (`europe-west3`), cross-referencing this as a live production deployment target.
impact: HIGH — Exposes internal Redis endpoint; aids lateral movement or targeted SSRF if any internal-facing service is reachable from an attacker-controlled context.
verify_steps: 1) Confirm `10.37.248.211` resolves from any sipgate internal network. 2) Verify `rediss://` (TLS) vs `redis://` by checking if TLS termination is enforced. 3) Check if this IP is still assigned in GCP project `clinq-services` zone `europe-west3`.
class: SECRET
asset: radau/docker-compose.yml:13-20
confidence: 80
reasoning: `docker-compose.yml` contains hardcoded plaintext values: `API_KEY_MANAGEMENT=pYWVcrR4DmgCfkfmEte5nGNW`, `API_KEY_RADIUS=u39fNShDX6fAeXtWY6bZWY9x`, `DB_PASSWORD=wifi`, `POSTGRES_PASSWORD=wifi`. The README confirms `radau` is a live WPA Enterprise authentication microservice ("Radius Authentication micro-service used to provide wifi logins"). While these appear to be dev/example values (README shows empty key placeholders for production), they are committed to a public repo and could match deployed dev instances. The `API_KEY_MANAGEMENT` key is used in `api/token.go:19` to gate all user/token management endpoints.
impact: MEDIUM — If any dev/staging instance uses these default keys, the management API (user create/delete/token management) is fully exposed. The WPA Enterprise RADIUS backend would also be accessible.
verify_steps: 1) Check if any running radau instances accept these API keys via `Authorization` header against `/user` or `/token` endpoints. 2) Verify the GCP project `clinq-services` (from clinq-bridge-sipgate) doesn't also host radau. 3) Test if the keys authenticate against the OpenAPI spec at `/docs/openapi.yml`.
class: SECRET
asset: rest-api-examples/webapp-nodejs/.npmrc.dist:2-3
confidence: 60
reasoning: The `.npmrc.dist` file contains `client_id=2414245-0-e24e0091-8265-11e7-93e7-e5fb754b756f` and `client_secret=187812ce-b546-4fa9-96e8-771e9775c3cb`. These follow the sipgate OAuth client credential format and could be real (revoked or still-valid) credentials. The README instructs users to copy this file to `.npmrc` (which is gitignored), but the `.dist` template ships the exact credential format. The `index.js` uses these values directly against `api.sipgate.com` OAuth endpoints.
impact: LOW-MEDIUM — If the sipgate OAuth server has not revoked this specific client credential, it could be used to obtain access tokens via the authorization code flow. Even if revoked, it reveals the exact credential format and naming convention for sipgate OAuth clients.
verify_steps: 1) Attempt an OAuth token exchange using the client_id/client_secret against `https://api.sipgate.com/login/third-party/protocol/openid-connect/token`. 2) Check if the client is listed in sipgate's developer portal. 3) Verify `.gitignore` properly excludes `.npmrc` (confirmed: `.gitignore` contains `.npmrc`).
class: MISCONFIG
asset: radau/main.go:33-37
confidence: 85
reasoning: When the `CORS_ORIGINS` environment variable is not set (the default), `initCORSConfig()` sets `corsConfig.AllowAllOrigins = true` alongside `corsConfig.AllowCredentials = true`. This means any origin can make credentialed cross-origin requests to the RADIUS auth API. The `Authorization` header is also explicitly allowed. This permits any website to perform API operations on behalf of authenticated users (token management, user CRUD).
impact: MEDIUM — An attacker-controlled page can perform cross-origin requests with the user's API key/JWT, enabling account takeover or data exfiltration if a user visits a malicious link while authenticated.
verify_steps: 1) Confirm the default path (no `CORS_ORIGINS` env) is the production configuration. 2) Verify the `AllowAllOrigins + AllowCredentials` combination is actually exploitable by testing a cross-origin request from `evil.com`. 3) Check if a reverse proxy (nginx/HAProxy) strips or overrides CORS headers before they reach clients.
class: OTHER
asset: rest-api-examples/webapp-nodejs/index.js:30
confidence: 70
reasoning: The Express session middleware uses `secret: 'sipgate-rest-api-demo'` — a hardcoded, publicly known signing key. This key is committed to a public GitHub repo. While this is clearly example/demo code, it establishes a pattern that could be copy-pasted into production code.
impact: LOW — Session fixation risk if used in production. In an example repo, the impact is limited to demonstrating an insecure pattern.
verify_steps: 1) Verify no production instances copy this exact secret. 2) Check if the session secret is overridable via environment variable (it is not in the current code).
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-03 21:53:14 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-03 23:44:21 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-04 02:22:48 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-04 07:24:35 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
