# Web Application & API Security (WAAS)

## Deployment Options

| Deployment Option | Integration Type | Description |
|-------------------|------------------|-------------|
| API Gateways (AWS API Gateway, Azure APIM, GCP Apigee, Kong) | Agentless | In-depth scans, analysis, and alerts for your APIs, integrating with Cortex API Security. |
| Workloads (Linux-based) | Agent-based (Beta) | Real-time detection and protection for web apps and APIs via Web and API Security profiles. |

!!! note "Linking APIs to business applications"
    To link APIs to business applications, two prerequisites must be met:

    - The VM must have the XDR agent with WAAS enabled.
    - Applications must be defined in Application Security Posture Management (ASPM).

## Agentless Ingestion (Third-Party Integration)

| Source | Documentation |
|--------|---------------|
| AWS API Gateway | [Configure API security from end to end](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Configure-API-security-from-end-to-end) |
| Azure APIM | [Ingest Azure APIM](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Ingest-Azure-APIM?tocId=zGIW~xav5iLolE8qUjuCkA) |
| GCP Apigee | [Ingest Apigee Proxy](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Ingest-Apigee-Proxy?tocId=DOUvOT67A7aHwhJPiztwbA) |
| Kong | [Ingest Kong](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Ingest-Kong?tocId=iCLN~7TdK9k9LEgyB68NSw) |
| F5 | [Ingest F5](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Ingest-F5?tocId=R_nfNfvqOHKWdxqkGGNEeA) |

## Agent-based Protection (Beta)

[Agent-based protection](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Agent-based-protection)

## API Specification Inventory

Cortex Cloud imports API specifications in OpenAPI format and also extracts them by scanning AWS and Azure API gateways. Specifications in the inventory are scanned for misconfigurations and vulnerabilities, and live traffic is validated against them to alert on deviations, undocumented endpoints, and security gaps.

[API specification inventory](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/API-specification-inventory)