# AWS Security Services Comparison Matrix

A guide comparing native AWS security tools to help architects and SecOps teams choose the right tool for the right job.



## Comparison Table

| Feature | **Amazon GuardDuty** | **Amazon Macie** | **Amazon Inspector** | **AWS Config** | **AWS Security Hub** | **Amazon Detective** | **AWS Audit Manager** | **AWS Security Lake** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Primary Goal** | **Threat Detection** | **Data Privacy** | **Vulnerability Mgmt** | **Compliance & Drift** | **Posture Mgmt (CSPM)** | **Investigation** | **Audit Readiness** | **Log Centralization** |
| **What it Does** | Detects malicious activity/unauthorized behavior. | Finds & protects PII/sensitive data in S3. | Scans EC2, ECR, & Lambda for CVEs. | Tracks config changes & resource history. | Central dashboard for security findings. | Root cause analysis & behavior graphs. | Automates compliance evidence collection. | Normalizes & stores security logs (OCSF). |
| **How it Works** | ML analysis of CloudTrail, VPC, & DNS logs. | Pattern matching & ML on S3 objects. | Agent-based or agentless scans of OS/code. | Continuous recording of resource snapshots. | Aggregates & checks vs. CIS/PCI standards. | Link analysis of historical log data. | Maps resources to compliance controls. | Converts logs to OCSF format in S3. |
| **Service Scope** | Regional | Regional | Regional | Regional | Regional (supports aggregation) | Regional | Regional (Multi-account) | Regional (Rollup support) |
| **Security Stage** | **Detection** | **Detection** | **Prevention/Detection** | **Assessment** | **Operations** | **Forensics** | **Compliance** | **Data Management** |
| **When to Use** | Baseline threat detection (malware, crypto). | Meeting GDPR/HIPAA requirements for S3. | Managing patches & software vulnerabilities. | Auditing config history & detecting drift. | Daily security operations & prioritization. | Post-incident deep dives & attack paths. | Preparing for formal audits (SOC2/PCI). | Long-term storage & 3rd-party SIEM. |

## Architectural Overview

To build a resilient AWS environment, these tools should be viewed as a layered defense:

1.  **Sensors (The Eyes):** *GuardDuty, Macie, Inspector, Config.* These tools generate the raw findings.
2.  **Operations (The Brain):** *Security Hub & Detective.* This is where you prioritize and investigate alerts.
3.  **Governance (The Memory):** *Audit Manager & Security Lake.* This is where you store history for auditors and long-term analysis.
