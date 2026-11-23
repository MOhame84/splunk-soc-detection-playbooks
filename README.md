# Splunk SOC Detection Playbooks

Starter Splunk searches and a sample Tier-1 escalation report for web attack and authentication investigations.

## Contents

- [`web_and_auth_detections.spl`](./web_and_auth_detections.spl): failed-login correlation, suspicious web requests, and 404 enumeration searches.
- [`soc_tier1_escalation_report.md`](./soc_tier1_escalation_report.md): a lab-simulation report showing evidence, assessment, and recommended follow-up.

## Before using a search

The examples assume fields such as `src_ip`, `clientip`, `uri`, `status`, and `EventCode`. Field names and indexes vary by sourcetype; adapt them to your CIM or data model, then validate each search against representative events. The login search finds failures and successes in the same search period and requires timeline review to confirm ordering.

The searches identify leads, not confirmed compromise. Check raw events and correlate with identity, application, proxy, and endpoint telemetry. Tune thresholds for normal traffic before creating production alerts.

## Safety and scope

The escalation report is a synthetic lab example. Replace its sample indicators and host details with verified evidence before using the format in a real incident. Follow your organization's incident-response and evidence-handling procedures.

