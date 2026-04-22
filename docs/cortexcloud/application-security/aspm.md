# Application Security Posture Management (ASPM)

## Overview

ASPM is included in the **Cloud Posture Management** license.

## Supported Data Sources

### Version Control Systems

| Data Source           | Type    |
|-----------------------|---------|
| AWS CodeCommit        | Cloud   |
| Azure DevOps          | Cloud   |
| Bitbucket Cloud       | Cloud   |
| Bitbucket Data Center | On-Prem |
| GitHub Cloud          | Cloud   |
| GitHub Enterprise     | On-Prem |
| GitLab SaaS           | Cloud   |
| GitLab Self Managed   | On-Prem |

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Onboard-Data-Sources)

### CI/CD Pipeline Systems

| Data Source | Type    |
|-------------|---------|
| CircleCI    | Cloud   |
| Jenkins     | On-Prem |

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Onboard-Data-Sources)

### CI Tools

| Data Source          | Notes      |
|----------------------|------------|
| AWS CodeBuild        |            |
| CircleCI             | Code scans |
| Cortex CLI           |            |
| GitHub Actions       |            |
| Jenkins              | Code scans |
| Terraform Cloud      | Run Tasks  |
| Terraform Enterprise | Run Tasks  |

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Onboard-Data-Sources)

### Private Package Registries

| Data Source       | Type    |
|-------------------|---------|
| JFrog Artifactory | On-Prem |

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Onboard-Data-Sources)

### Third-Party Security Scanners

| Data Source                        | Capabilities |
|------------------------------------|--------------|
| Semgrep                            | SCA & SAST   |
| Snyk                               | SCA & SAST   |
| SonarQube                          |              |
| Veracode                           |              |
| Generic 3rd Party AppSec Collector |              |

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Onboard-Data-Sources)
