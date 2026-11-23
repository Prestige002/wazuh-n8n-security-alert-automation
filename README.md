# Wazuh–n8n Security Alert Automation

An automated security monitoring and alert notification project integrating **Wazuh SIEM** with **n8n** to detect suspicious Windows authentication activity and deliver security alerts through multiple communication channels.

## Project Overview

This project demonstrates how security monitoring can be combined with workflow automation to improve alert delivery and reduce repetitive manual monitoring.

Wazuh detects repeated Windows authentication failures. n8n retrieves the alerts, filters them by severity, prevents duplicate notifications, and distributes qualifying alerts through Telegram, Gmail, and WhatsApp.

## Architecture

```text
Windows 10 Endpoint
       |
       v
Wazuh Agent
       |
       v
Wazuh SIEM
       |
       v
Custom Detection Rule 100100
       |
       v
n8n Scheduled Workflow
       |
       v
Retrieve Security Alerts
       |
       v
Format Alert Data
       |
       v
Duplicate Protection
       |
       v
Severity Filtering (Level 7+)
       |
       +----------+----------+
       |          |          |
       v          v          v
    Telegram    Gmail     WhatsApp
```

## Detection Logic

The project uses Windows Security Event ID **4625**, which records failed logon attempts.

A custom Wazuh rule identifies repeated authentication failures associated with the same username within a 60-second window.

| Parameter | Configuration |
|---|---|
| Rule ID | 100100 |
| Severity level | 10 |
| Event ID | 4625 |
| Frequency | 3 events |
| Timeframe | 60 seconds |
| Detection | Repeated failed authentication for the same username |

### Custom Wazuh Rule

```xml
<rule id="100100" level="10" frequency="3" timeframe="60">
  <if_matched_sid>60122</if_matched_sid>
  <same_field>win.eventdata.targetUserName</same_field>
  <description>Multiple Windows authentication failures for the same username within 60 seconds.</description>
  <group>authentication_failed,windows,brute_force,</group>
</rule>
```

## Automation Workflow

The n8n workflow performs the following operations:

1. **Scheduled monitoring:** Checks Wazuh alerts every minute.
2. **Alert retrieval:** Queries the Wazuh Indexer for relevant security events.
3. **Data processing:** Extracts agent details, rule information, severity, timestamps, and alert IDs.
4. **Duplicate protection:** Prevents previously processed alerts from triggering repeated notifications.
5. **Severity filtering:** Allows alerts with severity level 7 and above to proceed.
6. **Notification delivery:** Sends qualifying alerts through Telegram, Gmail, and WhatsApp.

## Technologies Used

- Wazuh SIEM
- n8n Workflow Automation
- Windows 10
- Ubuntu Linux
- Python (supporting security automation work)
- REST API and HTTP requests
- Telegram Bot API
- Gmail
- WhatsApp Cloud API
- VirtualBox

## Testing and Validation

The implementation was tested using repeated Windows authentication failures in a controlled lab environment.

Successful validation included:

- Detection of failed authentication activity.
- Generation of a high-severity Wazuh alert using rule 100100.
- Retrieval and processing of the alert by n8n.
- Successful delivery through Telegram, Gmail, and WhatsApp.
- Verification that subsequent scheduled executions did not resend the same processed alert.

## Security Considerations

- API tokens and credentials must never be committed to GitHub.
- Sensitive configuration values should be stored securely.
- Production deployments should use appropriate authentication, access controls, and secure network communication.
- Testing should be conducted only in authorized environments.

## Project Status

**Core implementation completed and tested.**

Further development will include improved documentation, workflow export, expanded alert investigation, and AI-assisted security analysis.

## Author

**Prestige002**

Electrical Engineering Graduate | Cybersecurity | AI & Automation Security Engineering

GitHub: [https://github.com/Prestige002](https://github.com/Prestige002)

---

*Building practical security solutions through automation, intelligence, and continuous learning.*

**Breaking the impossibles.**
