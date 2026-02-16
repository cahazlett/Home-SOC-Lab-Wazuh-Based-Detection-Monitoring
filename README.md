# Home SOC Lab – Wazuh-Based Detection & Monitoring

This repository documents the design, build, and operation of a home-based Security Operations lab focused on practical detection engineering and endpoint monitoring using open-source tooling.

The lab is treated as a working SOC environment — not a static demo. Changes are version-controlled, problems are documented, and architecture evolves over time.

---

## Objective

Build and operate a functional detection stack that mirrors entry-level SOC workflows:

- Deploy and maintain a SIEM platform (Wazuh)
- Enroll and monitor real endpoints
- Generate and validate alerts
- Troubleshoot configuration and connectivity issues
- Integrate additional telemetry sources (e.g., network IDS)
- Document decisions and trade-offs transparently

This is not a step-by-step tutorial. It is an operational build log.

---

## Lab Architecture (Current State)

**SIEM Platform**
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

**Primary Host**
- Ubuntu 24.04 LTS (SurfaceBook 2 dedicated as security node)

**Monitored Endpoints**
- Ubuntu workstation
- Windows 11 system

**Detection Expansion**
- Suricata IDS (AF_PACKET on WiFi interface)
- ET Open rule set
- Suricata `eve.json` integrated into Wazuh for centralized analysis

---

## What This Repository Tracks

- Architecture decisions and configuration changes
- Agent enrollment and troubleshooting
- Service failures and root-cause resolution
- Detection-layer expansion (endpoint + network)
- Activity logs organized chronologically

The goal is clarity and repeatability — not perfection.

---

## Documentation Structure
activity/
├── 2026-01-build-phase.md
├── 2026-02-layered-detection.md

Each file reflects work completed during that period, including:
- What changed
- Why it changed
- Issues encountered
- Validation steps

---

## Design Philosophy

- Start with endpoint visibility.
- Add layered detection deliberately.
- Break things. Fix them properly.
- Keep logs centralized.
- Treat the lab as production-grade infrastructure.

---

## Current Status

Active and expanding.

Next phases include:
- Additional detection tuning
- Network visibility improvements
- Alert quality refinement
- Investigation workflow documentation

Updates are added as work is completed.

