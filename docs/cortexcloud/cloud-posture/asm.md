# Cloud Attack Surface Management (ASM)

## Overview

ASM is included in the **Cloud Posture Management** license.

## Enable or Disable Cloud ASM

[Official Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Enable-Cloud-ASM)

1. Navigate to **Settings → Configurations → Attack Surface → Data Management**.
2. Toggle the **Enable Attack Surface Data** switch on or off.

## External Surface Assets

[Official Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/External-Surface-assets)

### Certificates

Tracks digital certificates used for encrypted communications (SSL/TLS, HTTPS, SSH, VPN). Collects issuer, public key, subject, SANs, and cryptographic health checks (self-signed, wildcard, expired, signature algorithm, key bits). Health checks appear as **Certificate Classifications** in asset details.

### Domains

Lists all domains attributed to your organization, including root domains and subdomains as separate entries. Wildcard DNS subdomains resolving to the same IP are grouped under one entry. Subdomains are collapsed under the parent if more than 1,000 are observed. Data is collected using active and passive global DNS scanning techniques.

### Services

Lists all internet-facing services (any device or software on a domain:port or IP:port pair responding over the public internet).

Key fields:

| Field                                       | Description                                                                                                                                                                                                                  |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Active Classifications**                  | Fingerprint-based identifiers of software, technologies, and behaviors (e.g., specific versions, configuration details, missing security headers).                                                                           |
| **Business Units**                          | Designation to classify assets and identify owning organizations (useful for subsidiaries / M&A).                                                                                                                            |
| **Discovery Type**                          | **Directly Discovered** — definitively associated with your organization (on-prem IP, your certificate, managed cloud resource). **Colocated with your Services** — running on the same IP as a directly-discovered service. |
| **Externally Inferred CVEs**                | Identified by matching product name/version against the National Vulnerability Database. Requires additional investigation to confirm.                                                                                       |
| **Externally Inferred Vulnerability Score** | Based on the highest CVSSv3 (or CVSSv2) score for inferred CVEs on the service.                                                                                                                                              |
| **Is Active**                               | Whether the service is currently observed on the internet.                                                                                                                                                                   |
| **Protocol**                                | Application-level protocol over which the service was validated.                                                                                                                                                             |