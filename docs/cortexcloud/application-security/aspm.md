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

## Code-to-Cloud

### Supported Integrations

| Component  | Supported Provider / Build Tool                                 |
|------------|-----------------------------------------------------------------|
| VCS        | GitHub, GitLab, Bitbucket, Azure DevOps                         |
| CI/CD      | GitHub Actions, GitLab CI/CD, Azure Pipeline, Jenkins, CircleCI |
| Containers | Docker CLI, docker compose, docker buildx, Kaniko               |
| VM Images  | AWS EC2 Image Builder, Packer (→ AWS AMI)                       |
| IaC        | Terraform (.tf), CloudFormation (.yml, .json)                   |

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Code-to-Cloud)

### Troubleshooting

#### Incomplete Lineage

| Issue                                        | Description                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Missing YOR Tags (IaC Resources)             | IaC resources without tags cannot be mapped to runtime. The system will prompt you to tag these resources.                                                                |
| Missing Pipeline Integrations (Repositories) | If pipeline integrations are missing, the link between code and build artifacts breaks.                                                                                   |
| Inactive Pipeline                            | Lineage is generated during pipeline runs. If a pipeline is integrated but has not run, trigger a build to generate the necessary artifacts and establish the connection. |

#### Required Integrations by Deployment Type

| Deployment Type                                  | Required Integration                           | Purpose                                                  |
|--------------------------------------------------|------------------------------------------------|----------------------------------------------------------|
| IaC (Terraform / CloudFormation)                 | YOR tags on IaC resources                      | Connects templates to real cloud resources               |
| CI/CD (GitHub Actions, GitLab CI, Jenkins, etc.) | CI/CD system integration                       | Links pipelines to code and build artifacts              |
| Containers                                       | Container registry (Docker Hub, ECR, GCR, ACR) | Shows built images and where they were deployed          |
| Cloud resources                                  | AWS / GCP / Azure cloud connectors             | Discovers runtime resources (VMs, containers, databases) |

#### Full Code-to-Cloud Connection Requirements

All of the following are needed for complete lineage:

```
VCS Repository
    ↓ YOR Tags on IaC
    ↓ CI/CD Pipeline Integration
    ↓ Container / Artifact Registry
    ↓ Cloud Connectors (AWS / GCP / Azure)
    = Assets visible in the application
```

To verify coverage, go to **ASPM Command Center → Coverage** and check for gaps.

## Applications

Applications are holistic entities that span the full application lifecycle from source code to cloud infrastructure. Each application is a logical, dynamic entity that groups:

- VCS repositories
- CI/CD pipelines
- Build artifacts (container images, VM images)
- Cloud infrastructure resources
- Runtime workloads

### Business Applications

Business Applications are a specific type of Application that adds business context to grouped assets. They allow you to define, group, and maintain assets with unified business meaning, correlate security risks across the development lifecycle, and prioritize security issues based on business impact.

### Creation Methods

| Method              | How It Works                                                                                                                          | Automation | Best For                                     |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------|------------|----------------------------------------------|
| VCS Criteria        | Dynamically groups assets based on VCS hierarchy (Organization, Project, Repository). New matching repos are onboarded automatically. | High       | Multiple apps, structured repos              |
| Cloud Tag Criteria  | Dynamically groups assets based on cloud tags (AWS, GCP, Azure, OCI). Maps infrastructure tags to application metadata fields.        | High       | Cloud environments, Code-to-Cloud visibility |
| Application Builder | Manual creation starting from a code or cloud asset. Cortex Cloud suggests related assets automatically.                              | Low        | Specific apps, granular control              |
| Public API          | Programmatic definition via API.                                                                                                      | High       | IaC, CI/CD pipelines, DevOps automation      |

### When to Use Each Method

| Use Case                                         | Recommended Method  |
|--------------------------------------------------|---------------------|
| Multiple apps, new repos onboarded automatically | VCS Criteria        |
| Infrastructure organized by cloud tags           | Cloud Tag Criteria  |
| Need strong Code-to-Cloud visibility             | Cloud Tag Criteria  |
| Few specific apps with fine-grained control      | Application Builder |
| Infrastructure-as-Code or CI/CD integration      | Public API          |
