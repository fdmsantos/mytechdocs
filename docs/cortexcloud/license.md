# License

For more information, see the [official license documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Understand-license-plans).

To view the product license and add-ons associated with your tenant, go to **Settings → Cortex License**.

## License Plans

| License | Configuration |
|---|---|
| **Cloud Posture Management** | Agentless comprehensive visibility across your cloud environment. Includes: CSPM, CIEM, ASPM, DSPM, AI-SPM, ASM, KSPM, CI/CD Posture Management, Agentless Workload Scanning |
| **Cloud Runtime Security** | Full cloud protection, detection, and response. Includes: Cloud Posture Management + CWP + Web Application & API Security (WAAS) |

## Add-ons

### Security Add-ons

Expand the core capabilities of Cloud Posture Management or Cloud Runtime Security:

- Data Ingestion
- Application Security (IaC Security, SCA, Secrets Security)
- Enterprise Runtime Security (XDR)
- Identity Threat Detection and Response (IDTR)
- Forensics Investigation
- Host Insights
- Extended Threat Hunting (XTH)
- Advanced Email Security
- Data Loss Prevention (DLP) — Beta

### Capacity Add-ons

Extend the duration that security and telemetry data are retained:

- **Data Retention** — Cortex Cloud Posture Management and Cloud Runtime Security retention per dataset
- **Query Capacity (compute units)** — A single Compute Unit add-on

## Protected Workloads

A workload represents any active compute entity that requires protection. These workloads count toward your license usage.

| Workload Type | Billable Units |
|---|---|
| VMs not running containers | 1 VM |
| VMs running containers | 1 VM |
| Endpoint | 1 Endpoint |
| CaaS (Container As A Service) | 10 Agent Protected Managed Containers |
| Cloud Buckets | 10 Cloud Buckets |
| Managed Cloud Database (PaaS) | 2 PaaS Databases |
| DBaaS TB Stored | DBaaS 1TB Stored |
| SaaS Users | 10 SaaS Users |
| On-Premise Data assets | 1 Connection |
| Cloud ASM – Service | 4 Unmanaged Assets |
| Container Images in Registries | Free quota: 10 image scans per deployed workload; beyond free quota: 10 container image scans |
| CLI Image Scans | — |

## License Usage & Overflow Rules

Cortex categorizes workloads as:

- **Cloud Posture Workloads** — workloads purchased with a Cloud Posture Management license, including security add-ons
- **Cloud Runtime Workloads** — workloads purchased with a Cloud Runtime license

A Cloud Runtime license includes both Posture Scanning and Runtime Protection on the same asset.

### Overflow Rules

| Licenses Purchased | Usage Scenario | Overflow Behaviour |
|---|---|---|
| Cloud Posture Only | Posture workloads exceed quota | All usage (including over-quota) counts toward the Posture license |
| Cloud Runtime Only | Runtime workloads exceed quota | All usage (including over-quota) counts toward the Runtime license |
| Both | Workloads within quota limits | No overflow; each counter tracks its own quota |
| Both | Posture exceeds quota, Runtime has remaining capacity | Spillover from Posture → Runtime (not the reverse) |
| Both | Runtime quota full | Excess Posture workloads are added back to the Posture counter |

## Data Retention

### Default Retention Periods

| Data Type | Default Retention Period |
|---|---|
| Ingested data | 31 days |
| Cases and Issues data | 186 days |
| Forensic data | 365 days (requires Forensics add-on) |
| Audit logs | 365 days |
| Query data | 186 days |

### Retention Add-ons

| Feature | Description |
|---|---|
| Additional Cases and Issues Retention | +31 days hot storage beyond the default 186 days, per endpoint per month |
| Period-Based Retention – Hot Storage (All datasets) | Fully searchable storage for ingested data and Cases/Issues data; requires minimum 1 month additional retention |
| Additional Hot Storage (Selected datasets) | Flexible hot storage per dataset; minimum 1,000 GB |
| Period-Based Retention – Cold Storage | Lower-cost storage for long-term compliance; minimum 6 months additional retention |