---

## February 2026 – Expanding the Home SOC (Wazuh + Suricata)

Over the last couple weeks I focused on stabilizing the Wazuh stack and adding network-based detection to move beyond basic endpoint logging.

This phase was less about installing tools and more about troubleshooting, validating configs, and making sure everything actually talks to each other.

---

### 1. Wazuh Dashboard Recovery

After some time away from the lab, the dashboard would not start.

Symptoms:
- Service failing on startup
- No HTTPS listener
- Browser could not connect

Root cause ended up being simple:  
The SSL certificate filenames in the dashboard config did not match the actual files on disk.

Fix process:
- Reviewed dashboard config
- Checked certificate directory
- Corrected filename mismatch
- Restarted service
- Verified HTTPS response locally

Lesson: Most “complex” failures are config mismatches. Check file paths before doing anything drastic.

Dashboard now running clean.

---

### 2. Adding a Second Linux Endpoint

Installed and configured a Wazuh agent on another Linux machine in the LAN.

Steps:
- Installed agent
- Set manager IP
- Enrolled agent
- Confirmed connectivity
- Verified logs appearing in dashboard

Now monitoring includes:
- auth logs
- syslog
- kernel logs
- package changes
- file integrity
- process activity

This moved the lab from single-host visibility to centralized monitoring across multiple endpoints.

---

### 3. Deploying Suricata (Network IDS)

To get network-level visibility, I deployed Suricata 7.x on the Wazuh server.

Initial issue:
Suricata kept crashing on startup.

Journal logs showed:
Interface not found.

It was configured to monitor `eth0`, but the system uses a different interface name.

Steps taken:
- Identified actual interface
- Updated suricata.yaml
- Tested config before restart:
  
  suricata -T -c /etc/suricata/suricata.yaml -v

After correcting the interface, the service started successfully.

---

### 4. Loading Detection Rules

Ran suricata-update to pull Emerging Threats Open rules.

Result:
~48k active rules loaded.

Confirmed rule file generation and successful config test before restart.

---

### 5. Integrating Suricata into Wazuh

To avoid having alerts isolated in separate systems, I configured Wazuh to ingest Suricata’s eve.json output.

Added:

<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>

Restarted services and confirmed Suricata alerts are now visible in Wazuh.

Now the stack includes:

- Host telemetry (HIDS)
- Network intrusion detection (NIDS)
- Centralized alert correlation

---

### Current Capabilities

- Multi-endpoint Linux monitoring
- Authentication tracking
- Kernel + system log ingestion
- File integrity monitoring
- Process visibility
- Network intrusion detection
- Centralized dashboard + alerting

---

### Architectural Notes

Current deployment runs over WiFi (client mode).  
That means Suricata sees traffic to/from the monitored host, not full LAN east-west traffic.

Future improvement:
Deploy a dedicated passive monitoring device (Raspberry Pi or similar) near the router for broader visibility.

---

### Key Takeaways from This Phase

- Always test config before restarting services.
- Never assume interface names.
- Centralize logs early.
- Layer detection sources.
- Confirm ingestion, don’t just assume it works.
- Troubleshooting is part of the build.

---

### Security Notes

- All services run on private LAN.
- No public exposure.
- No credentials, keys, or internal artifacts included in this repo.
- IP references sanitized where necessary.

---

This phase moved the lab from a basic SIEM install to a more realistic small-scale SOC-style detection stack with both endpoint and network visibility.

Next phase will focus on expanding network visibility and tuning detection.

---
