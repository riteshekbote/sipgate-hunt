# sipgate GmbH inventory (discovery seed 2026-09-02)
# NOTE: hosts below are discovery candidates from passive DNS/CT; confirm in-scope vs program scope before active testing.
app.sipgate.de
dev.sipgate.de
login.sipgate.de
mail.sipgate.de
sipgate.de
www.sipgate.de

## PASSIVE RECON 2026-09-02 (read-only, non-intrusive)

> Recon observations only. These are NOT confirmed vulnerabilities; ownership/in-scope of each host must be confirmed against the program scope before any active testing. Hosts resolve + serve HTTP — investigation requires scoped authorization.

**Probed:** 6 hosts | **Live HTTP:** 2

| Host | Status | Server/Tech |
|---|---|---|
| `app.sipgate.de` | 301 | Server: nginx -> https://login.sipgate.com/ |
| `login.sipgate.de` | 301 | Server: nginx -> https://www.sipgate.de |

**CNAME review signals (3):**
- `mail.sipgate.de` -> `ghs.google.com`
- `app.sipgate.de` -> `web-redirects.service.sipgate.net`
- `login.sipgate.de` -> `web-redirects.service.sipgate.net`

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `app.sipgate.de` | **Ports:** [80, 443]
**Web surface only:** [80, 443]

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `login.sipgate.de` | **Ports:** [80, 443]
**Web surface only:** [80, 443]

## 2026-09-02 19:21:52 UTC
