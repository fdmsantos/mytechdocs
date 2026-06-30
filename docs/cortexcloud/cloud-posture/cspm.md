# Cloud Security Posture Management (CSPM)

## Overview

CSPM is included in the **Cloud Posture Management** license.

## Cloud Workload

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
