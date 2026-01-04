# wazuh-agent-home-lab-aws


# Wazuh Agent Home Lab – AWS EC2 Ubuntu

## Background

This project is part of my career transition from **IT Support to IT Security**. The goal of this lab is to build a practical understanding of **endpoint security monitoring** using **Wazuh Agent** in a cloud-based home lab environment with minimal cost.

The lab focuses on configuring, validating, and testing a Wazuh Agent on an **AWS EC2 Ubuntu instance**, preparing it for future integration with a centralized SIEM (Wazuh Server & Dashboard).

---

## Environment

* Cloud Provider: AWS
* Service: EC2
* OS: Ubuntu Server (Free Tier)
* Security Tool: Wazuh Agent

---

## Objectives

* Deploy Wazuh Agent on a Linux endpoint
* Configure system and security log monitoring
* Enable File Integrity Monitoring (FIM)
* Validate agent functionality without a SIEM dashboard
* Capture real security events for analysis

---

## Implementation Steps

### 1. EC2 Preparation

* Launched an Ubuntu EC2 instance using AWS Free Tier
* Configured security group to allow SSH access
* Connected to the instance via SSH

---

### 2. Wazuh Agent Installation

* Installed Wazuh Agent using the official Wazuh repository
* Enabled and started the agent service

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

### 3. Log Monitoring Configuration

Configured Wazuh Agent to monitor critical system logs by editing:

```
/var/ossec/etc/ossec.conf
```

Enabled monitoring for:

* SSH authentication logs
* System logs

```xml
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/auth.log</location>
</localfile>

<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/syslog</location>
</localfile>
```

---

### 4. File Integrity Monitoring (FIM)

Enabled basic File Integrity Monitoring to track changes in system configuration files:

```xml
<syscheck>
  <disabled>no</disabled>
  <frequency>43200</frequency>
  <directories check_all="yes">/etc</directories>
</syscheck>
```

This allows detection of unauthorized changes to critical system files.

---

### 5. Agent Validation & Troubleshooting

Initially, the agent logs appeared empty. Troubleshooting steps included:

* Verifying the agent service status
* Ensuring monitored log files contained events
* Generating real SSH login failures
* Restarting the agent to reload configuration

After validation, the agent successfully began capturing logs.

---

### 6. Security Event Testing

Performed controlled tests to confirm log capture:

* Failed SSH login attempts
* Successful SSH login
* File creation and deletion under `/etc`

Verified captured events using:

```bash
sudo tail -f /var/ossec/logs/ossec.log

example logs

<pre>2026/01/04 13:06:47 wazuh-logcollector: INFO: (1950): Analyzing file: &apos;/var/log/dpkg.log&apos;.
2026/01/04 13:06:47 wazuh-logcollector: INFO: (1950): Analyzing file: &apos;/var/log/auth.log&apos;.
2026/01/04 13:06:47 wazuh-logcollector: INFO: (1950): Analyzing file: &apos;/var/log/syslog&apos;.
2026/01/04 13:06:47 wazuh-logcollector: INFO: Started (pid: 19410).
2026/01/04 13:06:47 wazuh-modulesd: INFO: Started (pid: 19424).
2026/01/04 13:06:47 wazuh-modulesd:agent-upgrade: INFO: (8153): Module Agent Upgrade started.
2026/01/04 13:06:47 wazuh-modulesd:ciscat: INFO: Module disabled. Exiting...
2026/01/04 13:06:47 wazuh-modulesd:osquery: INFO: Module disabled. Exiting...
2026/01/04 13:06:47 sca: INFO: Module started.
2026/01/04 13:06:47 wazuh-modulesd:control: INFO: Starting control thread.
2026/01/04 13:06:47 sca: INFO: Loaded policy &apos;/var/ossec/ruleset/sca/cis_ubuntu22-04.yml&apos;
2026/01/04 13:06:47 sca: INFO: Starting Security Configuration Assessment scan.
2026/01/04 13:06:47 wazuh-modulesd:syscollector: INFO: Module started.
2026/01/04 13:06:47 wazuh-modulesd:syscollector: INFO: Starting evaluation.
2026/01/04 13:06:47 sca: INFO: Starting evaluation of policy: &apos;/var/ossec/ruleset/sca/cis_ubuntu22-04.yml&apos;
2026/01/04 13:06:49 wazuh-modulesd:syscollector: INFO: Evaluation finished.
2026/01/04 13:06:49 wazuh-logcollector: INFO: (9203): Monitoring journal entries.
2026/01/04 13:06:51 wazuh-agentd: INFO: Requesting a key from server: 127.0.0.1
2026/01/04 13:06:51 wazuh-agentd: ERROR: (1208): Unable to connect to enrollment service at &apos;[127.0.0.1]:1515&apos;
</pre>


```

---

## Results

* Wazuh Agent successfully captures:

  * SSH authentication events
  * System log events
  * File integrity changes
* Agent runs persistently and starts automatically on boot
* Endpoint is fully prepared for centralized SIEM integration

---

## Skills Demonstrated

* Linux log analysis
* Endpoint security monitoring
* SIEM agent deployment and validation
* Cloud-based security lab setup (AWS EC2)
* Troubleshooting security tooling

---

## Next Steps

* Deploy Wazuh Server & Dashboard on Oracle Cloud (Always Free)
* Connect EC2 agent to centralized SIEM
* Enable alerting and dashboards
* Simulate security incidents and analyze alerts

---

## Disclaimer

This project is intended for **learning and portfolio purposes** and follows cloud free-tier best practices to avoid unnecessary costs.

