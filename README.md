# LinuxServer-it-operations-lab

Hands-on Linux IT operations lab focused on server administration, Docker containerization, networking, monitoring, incident response, and automation.

## Completed work

| Component | Demonstrated work | Documentation |
|---|---|---|
| Server infrastructure | Ubuntu Server installation, SSH administration, DHCP reservation, network verification | [Setup](LinuxServer-Lab/Infrastructure/Server-Setup/Server-Setup.md) |
| Container platform | Docker official APT repository, GPG key verification, Engine/Compose installation, hello-world test | [Docker](LinuxServer-Lab/Infrastructure/Docker/Docker.md) |
| Availability monitoring | Uptime Kuma deployment with persistent volume, SSH tunnel, TCP/HTTP monitors, shared bridge network | [Uptime Kuma](LinuxServer-Lab/Monitoring/uptime-kuma/Uptime-kuma.md) |
| Incident testing | Dockerized Nginx, HTTP validation, controlled outage and recovery, network configuration | [Nginx test lab](LinuxServer-Lab/Testing/Service-Outage-Test/Outage-Test.md) |
| Incident reporting | Documented incident timeline, root cause, resolution, and recommendations | [INC-001](LinuxServer-Lab/incident-reports/INC-001-nginx-outage.md) |

## Architecture at a glance

```text
Windows administrator workstation
       |
       | SSH / SSH local port forward
       v
Ubuntu Server (kmserver, Dell OptiPlex 3060)
       |
       +-- Docker Engine / Compose
             |
             +-- Uptime Kuma ----+  lab-monitoring (bridge)
             |                   |
             +-- Nginx test ----+
```

**Current scope:** A personal, controlled home-lab environment. Planned future additions (not yet implemented) include performance metrics, centralized application data, automated ticketing, and scripted workflows.

## Reproducibility and security

- Deployment examples are stored as Docker Compose YAML files and supporting documentation.
- Local-only published HTTP ports are bound to `127.0.0.1`; remote browser access to Uptime Kuma uses SSH port forwarding.
- The shared bridge network must be created before deploying services that refer to it as `external: true`.
- No live passwords, private keys, `.env` values, or application data volumes are included. Review screenshots for local identifiers before publishing.
