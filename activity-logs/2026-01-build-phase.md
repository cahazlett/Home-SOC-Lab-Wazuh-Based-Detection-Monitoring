# 2026-01 – Initial SOC Build Phase

**Date completed:** 2026-01-XX  
**Objective:** Stand up a functional Wazuh-based monitoring stack and onboard initial endpoints.

---

## Summary

January focused on establishing a working foundation for the home SOC lab.  
The goal was simple: deploy Wazuh, enroll endpoints, validate telemetry, and confirm visibility across systems.

This phase was less about advanced detection and more about building a stable monitoring base that future layers could rely on.

---

## Architecture at This Stage

• Wazuh server deployed on Ubuntu (SurfaceBook host)  
• Wazuh dashboard configured and accessible over LAN  
• Windows 11 endpoint enrolled  
• Ubuntu endpoint enrolled  
• Default rule set enabled  

No external sensors or network IDS were deployed at this point.

---

## Implementation Notes

### Wazuh Deployment

Wazuh components (manager, indexer, dashboard) were installed and verified as active via systemd.  
Dashboard SSL configuration required certificate path corrections before service stability was achieved.

Service validation included:

- systemctl status checks
- Port validation
- Agent connectivity confirmation

---

### Agent Enrollment

Endpoints were enrolled using the Wazuh agent installer with manager IP configured manually.

Validation steps included:

- Confirming agent "active" status in dashboard
- Verifying log ingestion from:
  - `/var/log/auth.log`
  - `/var/log/syslog`
  - Windows Security logs

---

## Challenges Encountered

1. Dashboard service did not initially bind to expected port.
2. SSL certificate naming mismatch prevented startup.
3. Agent connection troubleshooting required manual log review.

Each issue was resolved by validating configuration paths and confirming service dependencies were running correctly.

---

## Outcome

At the end of January:

• Wazuh was fully operational.  
• Two endpoints were actively reporting.  
• Log ingestion from Linux and Windows was confirmed.  
• Alerts were visible in the dashboard.  

The lab had baseline host visibility, but no network-level telemetry.

---

## Lessons Learned

• Service status alone is not proof of functionality — port checks matter.  
• Certificate path mismatches are common and must be verified explicitly.  
• Agent logs provide faster troubleshooting than dashboard UI.  
• Build stable foundations before adding detection layers.

---

## What This Enabled

This baseline made it possible to expand into layered detection in February, including:

• Network IDS deployment  
• Suricata integration  
• JSON log ingestion  
• Cross-source event correlation  

---

**Skills demonstrated:**

• SIEM deployment and validation  
• Endpoint log onboarding  
• Linux service troubleshooting  
• SSL configuration correction  
• Basic SOC workflow validation  
