# Incident Report INC-001

**Project:** LinuxServer IT Operations Lab  
**Incident title:** Nginx Web Service Outage  
**Classification:** Simulated service availability incident  
**Affected system:** `lab-nginx`  
**Status:** Resolved  
**Priority:** Low — planned exercise  
**Detection:** Uptime Kuma HTTP monitoring  
**Environment:** Docker test container, not production

## 1. Incident summary

A controlled service outage was conducted against a Docker-hosted Nginx web server to validate availability monitoring and incident detection in the IT Operations Lab. The Nginx container was intentionally stopped, causing HTTP service monitoring to fail. Uptime Kuma reported a DOWN condition. After the Nginx container was restarted, Uptime Kuma reported UP and confirmed an HTTP `200 - OK` response.

## 2. Timeline

| Event | Local time (2026-11-08) |
|---|---|
| Nginx container stopped | 18:33:00 |
| Uptime Kuma detected DOWN | 18:33:28 |
| Nginx container restarted | 18:35:00 |
| Uptime Kuma confirmed UP | 18:35:28 |

**Detection delay:** 28 seconds from container stop to monitoring detection.  
**Container stopped interval:** 2 minutes.  
**Observed incident window:** 2 minutes 28 seconds from container stop to monitoring-confirmed recovery.  

These are recorded timeline values, not independently measured service-response times. In particular, HTTP availability may have recovered before Uptime Kuma's next successful check.

## 3. Detection and impact

- **Monitor:** HTTP(s) availability check of `http://lab-nginx:80` through Uptime Kuma.
- **Check interval:** 60 seconds.
- **Observed condition:** Monitor transitioned from UP to DOWN, displaying `ENOTFOUND lab-nginx` while the container was stopped, then returned to UP after restoration.
- **Impact:** Test HTTP endpoint unavailable to the monitoring client. No production users or systems were affected.


## 4. Root cause

The service interruption was intentionally initiated as a planned test through Docker container administration:

```bash
sudo docker stop lab-nginx
```

The container stopped and Docker no longer provided the monitored Nginx HTTP endpoint. This was not an unexpected application crash or external cyberattack.

## 5. Remediation and validation

Restored the test container using:

```bash
sudo docker start lab-nginx
```

Checked running containers with `sudo docker ps` and observed Uptime Kuma return the endpoint to UP with an HTTP success response.


## 6. Lessons learned

- HTTP availability monitoring successfully observed the controlled interruption and recovery.
- The shared Docker network allowed an isolated monitoring container to reach the test service by name.
- Container state and monitor state should both be checked to verify recovery.
- A planned test should be clearly distinguished from a real production incident in documentation.

## 7. Recommended improvements

- Integrate outage alerts with automated incident ticket creation.
- Add CPU, memory, and disk metrics using a monitoring stack such as Prometheus and Grafana.
- Measure detection and recovery times systematically in later tests.
- Consider external monitoring for host-level outages, which a monitor running on the same host cannot detect while offline.

## 8. Final outcome

**PASS —** Uptime Kuma detected the intentional service interruption; Docker was used to restart Nginx; Uptime Kuma confirmed service restoration. See [full lab walkthrough](https://github.com/stealthdm43/LinuxServer-it-operations-lab/blob/main/LinuxServer-Lab/Testing/Service-Outage-Test/Outage-Test.md).
