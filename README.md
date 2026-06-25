# Splunk Windows Authentication Monitoring and Brute Force Detection Lab

## Overview

This project demonstrates the deployment of Splunk Enterprise and Splunk Universal Forwarder to collect and analyze Windows Security Event Logs. The lab focuses on authentication monitoring, failed login detection, dashboard creation, alerting, and investigation of potential brute-force activity.

## Tools Used

- Splunk Enterprise
- Splunk Universal Forwarder
- Windows 11
- SPL (Search Processing Language)

## Data Sources

Windows Security Event Logs:

- Event ID 4624 – Successful Logon
- Event ID 4625 – Failed Logon

## Objectives

- Ingest Windows Security logs into Splunk
- Detect failed authentication attempts
- Monitor authentication activity through dashboards
- Configure alerts for suspicious login behavior
- Investigate authentication-related security events

## Dashboard Panels

1. Failed Login Attempts by Account
2. Failed Login Activity Over Time
3. Successful vs Failed Login Activity
4. Authentication Activity by Logon Type

## Detection Queries

### Failed Login Detection

```spl
index=* EventCode=4625
```

### Failed Login Attempts by Account

```spl
index=* EventCode=4625
| stats count by Account_Name
| sort - count
```

### Successful vs Failed Login Activity

```spl
index=* (EventCode=4624 OR EventCode=4625)
| eval LoginStatus=if(EventCode=4624,"Successful Login","Failed Login")
| stats count by LoginStatus
```

### Authentication Activity by Logon Type

```spl
index=* (EventCode=4624 OR EventCode=4625)
| stats count by Logon_Type
```

### Potential Brute Force Detection

```spl
index=* EventCode=4625
| stats count by Account_Name
| where count > 5
```

## Alert Configuration

**Alert Name:** Possible Brute Force Login Attempt

- Trigger Condition: Number of Results > 0
- Schedule: Every Hour
- Action: Add to Triggered Alerts
- Severity: Medium

## Investigation Summary

Multiple failed authentication attempts were generated in a controlled lab environment and successfully detected using Splunk searches and alerting logic.

The investigation included:

- Reviewing Event ID 4625 failed logons
- Identifying targeted accounts
- Analyzing authentication trends
- Reviewing logon types
- Comparing successful and failed authentication activity

## Results

- Successfully ingested Windows Security Event Logs into Splunk.
- Generated and detected failed authentication events.
- Built authentication monitoring dashboards using SPL.
- Configured a threshold-based alert for potential brute-force activity.
- Performed event analysis and investigation using Splunk Search Processing Language (SPL).

## Screenshots

Add screenshots in a folder named:

```text
screenshots/
├── splunk_log_ingestion.png
├── failed_login_detection.png
├── failed_login_attempts_by_account.png
├── failed_login_attempts_over_time.png
├── brute_force_detection_search.png
├── brute_force_alert_configuration.png
├── successful_vs_failed_logins.png
├── authentication_activity_by_logon_type.png
└── windows_authentication_monitoring_dashboard.png
```

## Skills Demonstrated

- SIEM Administration
- Log Management
- Windows Event Analysis
- SPL Query Development
- Security Monitoring
- Threat Detection
- Alert Configuration
- Incident Investigation
