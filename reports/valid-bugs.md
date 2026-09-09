# Validated findings (running count 0)

- 10 lead(s) marked VALID at 2026-09-05 23:46:20 UTC
  - **Verdict: VALID** (already accepted in KB)
  - **Verdict: VALID** (already accepted; write path still open)
  - **Verdict: VALID** (already accepted)
  - **Verdict: VALID** (already accepted; chain-dependent)
  - **Verdict: VALID** (already accepted; deployment confirmation needed for full impact)
  - | 1 | API v2 CORS arbitrary-origin + credentials | **VALID** | 5.3–8.1 | Already in KB |
  - | 2 | Desktop-app S3 bucket (listable + write?) | **VALID** | 5.3–9.8 | Write path open (HUMAN) |
  - | 3 | Dev SPA internal infrastructure disclosure | **VALID** | 5.3 | Already in KB |
  - | 5 | GitHub Redis IP + GCP project | **VALID** | 5.3 | Chain-dependent (needs SSRF) |
  - | 6 | Radau CORS + hardcoded secrets | **VALID** | 4.3–7.5 | Deployment unconfirmed |

- 1 lead(s) marked VALID at 2026-09-09 11:38:09 UTC
  - | **share1.sipgate.cloud dangling CNAME** | **VALID** | Novel subdomain takeover candidate. CNAME → `nx38603.your-storageshare.de` → NXDOMAIN. Provable via `dig`. In-scope (sipgate.cloud). CVSS 6.5 ME
