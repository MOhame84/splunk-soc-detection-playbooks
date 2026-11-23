# SOC Tier-1 Incident Escalation Report (Lab Simulation)

## 1. Alert Metadata
- **Incident ID:** INC-2026-1123-04
- **Alert Name:** Path Traversal Payload Returning HTTP 200 OK
- **Severity:** High
- **Assessment:** **HIGH PRIORITY — potential unauthorized file read; validate response content**
- **Source IP:** `10.10.14.88`
- **Target Host:** `web-prod-01` (`192.168.10.5`)

## 2. Timeline & Evidence Observed
- **08:12:05 – 08:12:09 UTC:** Source IP `10.10.14.88` generated 5 rapid `HTTP 404` requests (`/admin`, `/backup`, `/config`, `/server-status`, `/.env`) using User-Agent `gobuster/3.6` (Reconnaissance phase).
- **08:12:15 UTC:** The same source IP switched to a browser User-Agent (`Mozilla/5.0`) and issued:
  `GET /image?filename=../../../../etc/passwd HTTP/1.1`
- **Server Response:** Returned `HTTP 200 OK` with response size `1482 bytes` (significantly larger than standard error responses).

## 3. Correlation & Analysis
- **Evidence:** Sequential directory enumeration followed by a traversal-shaped request to `/image`; the server returned `HTTP 200` and 1482 response bytes.
- **Interpretation:** This is consistent with possible file disclosure, but status and response size alone do not confirm what content was returned. Review the response/body evidence and application telemetry.
- **Ruled Out (Benign):** Not a web crawler or broken link due to the explicit `../` traversal sequence and `gobuster` User-Agent preceding the request.

## 4. Recommended Containment & Remediation Actions
1. **Containment decision:** Follow incident policy; consider temporary blocking or rate limiting if activity is ongoing, after checking whether the source is an authorized scanner.
2. **Threat Hunting:** Query Splunk across the past 7 days for any other source IPs sending `filename=*..*` to confirm whether this flaw was previously exploited.
3. **Engineering Fix:** Canonicalize the requested path and enforce that the resolved file remains inside the intended upload directory. Verify with regression cases for encoded and double-encoded traversal.

> Lab simulation only. The addresses and hostnames are illustrative; replace them with verified evidence before adapting this template to a real incident.

