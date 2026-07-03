# Cloud Attack Surface Management (ASM)

## Overview

ASM is included in the **Cloud Posture Management** license.

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Cloud-ASM)

## Enable or Disable Cloud ASM

[Official Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Enable-Cloud-ASM)

1. Our scanning activity on the ranges below is CFAA-compliant. Mark our ranges as non-malicious in your system so that you stop getting alerts, or configure your firewall to drop traffic from our ranges ([Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Scanning-activity)).
2. Navigate to **Settings → Configurations → Attack Surface → Data Management**.
3. Toggle the **Enable Attack Surface Data** switch on or off.

## Externally Inferred CVEs

CVEs are inferred by matching a service's reported version against the NVD, with three confidence levels:

| Confidence | Description |
|------------|-------------|
| High | Exact version match against the NVD. Cortex Cloud generates an issue for investigation (e.g., service reports Apache 2.4.49 and the CVE affects exactly 2.4.49). |
| Medium | Partial version match with extra characters (e.g., Apache 2.4.49c vs. 2.4.49). Cortex Cloud creates a finding, not an issue, since confidence is lower and further investigation is required. |
| No Match | The service version differs from the affected version (e.g., service runs Apache 2.4.50 but the CVE affects 2.4.49). |

