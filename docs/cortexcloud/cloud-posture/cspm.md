# Cloud Security Posture Management (CSPM)

## Overview

CSPM is included in the **Cloud Posture Management** license.

## Cloud Security

Documentation: [Cloud Security Rules and Policies](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Cloud-Security-Rules-and-Policies)

Cloud security rules and policies define and manage security guardrails consistently across AWS, Azure, GCP, and other cloud providers. They detect specific conditions in target environments, generating findings and issues for misconfigurations and threats.

### Rules

Cloud security rules define conditions that apply to cloud, code, or host resources. They use detection logic or XQL queries to identify threats or misconfigurations by examining asset configuration attributes. Rules are evaluated against all matching assets, generating findings when matching criteria are met.

All rule types share common fields: **Name**, **Description**, **Severity** (inherited by findings), **Labels** (optional), and optional **Remediation** instructions.

#### Rule Types

##### Attack Path

Documentation: [Create an Attack Path Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-and-manage-cloud-security-rules)

Identifies critical risks from combinations of risk signals — overly permissive identities, network exposures, and vulnerabilities — that together form a potential breach path to high-value assets.

**Rule Logic:** Select a primary asset, then attach **Finding** and/or **Vulnerability** conditions. The rule triggers on the intersection of any selected findings AND the vulnerability on the asset (not all conditions need to be present simultaneously).

##### Configuration

Documentation: [Create a Configuration Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-a-configuration-rule)

Monitors resource configurations for misconfigurations and policy violations. Supports two modes:

- **Simple:** Guided dropdowns for common conditions.
- **Advanced:** Free-form XQL queries against the `asset_inventory` dataset. Conditions use `xdm.asset.raw_fields` to access configuration JSON. Requirements: asset type must be specified via `xdm.asset.type.id`, output must include `asset_id` and `asset_type_id`, maximum 10 output fields, `fields` stage must be last.

Supports optional association with **custom compliance controls**.

##### Data

Documentation: [Create a Data Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-a-data-rule)

Detects malware and classifies sensitive data across cloud storage assets (databases, disks, buckets).

**Rule Logic:** Select an asset category (database, disk, bucket), add WHERE conditions on asset attributes, then attach Finding conditions (Configuration Finding, Data Finding, Identity Finding, etc.) with their own WHERE conditions.

##### Identity

Documentation: [Create an Identity Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-an-identity-rule)

Monitors cloud identities for excess or unused permissions.

##### Network Exposure

Documentation: [Create a Network Exposure Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-a-network-exposure-rule)

Detects assets exposed to unrestricted public internet access. Powered by the **Cloud Network Analyzer (CNA)**.

##### AI

Documentation: [Create an AI Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-an-AI-rule)

Detects misconfigurations and security flaws across AI infrastructure, supply chains, and data models for AWS Bedrock, Amazon SageMaker, Azure OpenAI, and GCP Vertex AI.

**Rule Logic:** Select AI asset categories (Dataset, AI Model, Model Endpoint), add WHERE conditions to define risk criteria (e.g., models trained on sensitive data buckets, models with public exposure).

Supports optional association with **custom compliance controls**.

### Policies

Cloud security policies define the **scope** (which assets a rule applies to) and **enforcement** (what happens when a rule triggers), complementing the detection logic provided by rules.

A policy consists of:

- **Rules:** Select existing detection rules or create new ones.
- **Scope:** Filter which assets the rule applies to.

Rules alone generate findings across all assets. When associated with a policy, findings within the policy scope are promoted to **issues**.

> **Note:** With SBAC enabled (`User Settings → Cases and Issues Scope → Posture`), Issues/Cases/Findings counts may diverge. The Rules page counts Cases in the Posture domain; Platform pages count Issues within Cases in the Posture domain.

## Cloud Workload

Documentation: [Cloud Workload Policies and Rules](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Cloud-Workload-Policies-and-Rules)

Cloud Workload Policies prevent and manage security violations in cloud runtime instances by applying detection logic to specific asset groups at the desired SDLC stage, and defining the action to take when conditions are met.

### Policies

A Cloud Workload Policy is composed of the following elements:

- **SDLC Evaluation Stage:** The stage at which the policy is applied:
    - **CI:** During pipeline build, before pushing the artifact to a registry.
    - **Deploy:** When the artifact is pushed to a cloud instance.
    - **Runtime:** When the artifact is running on a cloud instance. The Prevent action at this stage applies only to Kubernetes Workload Images assets.
- **Rule (Conditions):** Logical conditions that trigger the policy evaluation.
- **Scope:** Filter defining which assets the rule applies to.
- **Action:** Response triggered when the rule evaluates successfully — can create an issue or block the security violation.

#### Policy Types

##### Misconfiguration

**SDLC Evaluation Stage:** Runtime

Assess workloads for misconfigurations against security standards and organizational guidelines. Supports both predefined and custom rules, and can either prevent violations or create issues.

##### Malware

**SDLC Evaluation Stage:** Runtime, CI, Deploy

Detect and manage malicious files within cloud workloads. Analyzes files based on predefined parameters such as name, path, size, and detection method.

##### Secret

**SDLC Evaluation Stage:** Runtime, CI, Deploy

Identify and protect sensitive information — such as API keys and credentials — within workloads.

##### Trusted Image

Documentation: [Trusted image cloud workload policies](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Trusted-image-cloud-workload-policies)

**SDLC Evaluation Stage:** Runtime

**Scope (Asset Groups):** The policy applies only to cloud workload asset types available at the Runtime stage — container images, container instances, hosts (VM instances), serverless functions, or Kubernetes workloads — that belong to the selected asset groups.

Ensure the authenticity, integrity, and security of container images and VMs deployed into Kubernetes environments. Includes actions such as limiting allowed image sources and mitigating image tampering.

**Trust Verdict Unavailable:**

When insufficient information exists at deployment time, Cortex cannot determine if an image is trusted or untrusted. Common causes:

- **Missing scan data:** First-time evaluation where data from a prior scan (e.g. base layer info, CLI scan status) is not yet available.
- **Partial deployment metadata:** The deployment file omits metadata required by the policy (e.g. policy requires a tag, but the Kubernetes resource references only the image digest/SHA).

**Decision Logic:**

- **Issue-only policies:** Any image that fails the trust criteria generates a Posture Issue (non-blocking; the image is allowed despite the finding).
- **Prevent policies:**
    - **Conflicting allow/block policies:** If multiple policies apply to the same image and one allows while another blocks, the image is allowed (*Allow-Wins* principle).
    - **Mixed images in the same workload:** If a workload contains both trusted and untrusted images, the entire deployment is blocked (a single untrusted image prevents execution).

#### Preventive Action

Some policies support a **Prevent and Create an Issue** action that enforces compliance during deployments.

**Runtime Stage:**

- Prevention applies only to **Kubernetes Workload Images** assets via the Kubernetes Admission Controller (requires KSPM Connector with Admission Control enabled on the cluster).
- For all other asset types in scope, prevention is skipped and an Issue is created instead.
- Managed from the Kubernetes Connectivity Management page (`/cwp/k8s-management`).

**CI Stage:**

- Prevention triggers a pipeline failure by returning **exit code 2** in the CI tool.

> **Recommended approach:** Start with *Create an Issue* to validate results before switching to *Prevent and Create an Issue*, to avoid disrupting deployments.
> Prevention only affects **new or future** deployments — already-running assets are not impacted.

### Rules

Cloud Workload Rules define the criteria for identifying security violations, applied to assets and findings in your cloud environment. Rules alone only enable detection — they must be included in a policy to trigger a preventive response or generate an issue.

**Scanners:**

- **Agentless Disk Scan:** Inspects container images using the Agentless Disk Scanner. Supports OS-specific rules (e.g. checking for malicious entries in `etc\hosts` on Windows images).
- **Kubernetes Connector:** Inspects Kubernetes environment variables and resources such as Namespaces, ReplicaSets, and Deployments.
- **XDR Agent:** Performs custom compliance checks by executing user-defined Python scripts via the XDR Agent Scanner.

## Containers

### Image Types

| Image Type | Description | Discovery Method | Key Properties |
|---|---|---|---|
| **Core Image** | Immutable content of the image; foundation for all other types. | — | Identified by SHA256 digest. Contains file-related findings (vulnerabilities, secrets, malware). Can reference another Core Image as its base. No scope — cannot belong to an asset group or policy directly. |
| **Build Image** | Image produced by a CI/CD pipeline or build process. | CLI scanning | Includes build metadata (build time, source repo, build environment). Contains build-specific findings and issues. Represents a Core Image. |
| **Registry Image** | Image stored in a container registry (AWS ECR, Azure ACR, Google GAR, JFrog Artifactory, Docker). | Cloud discovery or registry scanning | Includes registry metadata (FQDN, repository name, tags, manifest digests). Resides within an image repository. Represents a Core Image. |
| **Runtime Image** | Image stored, running, or defined in a workload (VMs, Kubernetes workloads). | Agentless Disk scan / XDR agent scan | Contains deployment and operational findings (config deviations, policy violations). File-related findings are derived from the connected Core Image. Represents a Core Image. |

All image types can be queried via XQL and are listed under **Inventory → All Assets → Compute → Container Images**.

### Base Images Rule

Documentation: [Base Images Rule](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Base-Images-Rule)

Defines which registry images are considered foundational base images in the organization, creating `BASE_REFERENCE` relations between images for bidirectional lineage tracing.

**Capabilities:**

- Identify the base image for any given image
- View all dependent images derived from a specific base image
- Identify affected base images during vulnerability investigations
- Use base image associations in policies, queries, and filters

**Filter conditions:** Registry URL, Repository name, Image Name, Image Tag (supports Equals, Not Equals, Contains, Not Contains, Starts With, Ends With).

Rules can be created from **Posture Management → Rules & Policies → Rules → Base Images** or directly from a Registry Image asset card (**Inventory → Assets → All Assets → Compute → Container Images** → More options → *Add base image rule*).

> Changes take up to **6 hours** to propagate across assets.
