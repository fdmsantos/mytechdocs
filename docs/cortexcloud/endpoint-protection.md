# Endpoint Protection (XDR)

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Endpoint-security)

[Create an Agent Installation Package](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-an-agent-installation-package)

[Configure Global Agent Settings](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Configure-global-agent-settings)

## Policy Management

### Supported Platforms

| Platform            | Profiles                                                          |
|----------------------|--------------------------------------------------------------------|
| Windows              | Malware, Exploit, Agent Settings, Restrictions, Exceptions         |
| macOS                | Malware, Exploit, Agent Settings, Restrictions, Exceptions         |
| Linux                | Malware, Exploit, Agent Settings, Restrictions, Exceptions         |
| Android              | Malware, Agent Settings                                            |
| iOS                  | Malware, Agent Settings                                            |
| CaaS                 | Malware, Exploit, Agent Settings, Restrictions, Exceptions         |
| Serverless Function  | Restrictions                                                        |

### Harden Endpoint Security

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Harden-endpoint-security)

| Module          | Description                                                                                                                                                                                     | Windows                                                                                | Mac                                                                                    | Linux | Details |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|-------|---------|
| Device Control  | Protects endpoints from loading malicious files from USB-connected removable devices (CD-ROM, disk drives, floppy disks, and Windows portable devices drives) and Bluetooth devices. Protects endpoints from malicious print jobs. | ✓ (Bluetooth from Cortex XDR agent version 8.6, print jobs from version 8.5)            | ✓ (Bluetooth from Cortex XDR agent version 8.7, print jobs from version 8.5)            | –     | Supports Exception Profiles and Custom Device Classes, configured at: Endpoints → Policy Management → Settings → Device Management |
| Host Firewall   | Protects endpoints from attacks originating in network communications to and from the endpoint.                                                                                                | ✓                                                                                          | ✓                                                                                          | –     |         |
| Disk Encryption | Provides visibility into endpoints that encrypt their hard drives using BitLocker or FileVault.                                                                                                | ✓                                                                                          | ✓                                                                                          | –     |         |
| File Integrity Monitoring | Monitors changes to critical files and directories on endpoints.                                                                                                                     | ✓                                                                                          | –                                                                                          | ✓     |         |

### Exception and Exclusion

| Type                                     | Description                                                                                                    |
|-------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| All Issue Exception & Exclusion Rules     | Define criteria to suppress (ignore) specific alerts/issues. Can be either **Issue Exception rules** or **Exclusion Rules** |
| IOC/BIOC Suppression rules                | Exclude one or more indicators from an IOC/BIOC rule that triggers actions on specific behaviors                 |
| Disable Injection and Prevention rules    | Create exceptions that exempt a process from the injection and prevention modules                                |
| Disable Prevention rules                  | Define granular exceptions for prevention actions triggered on endpoints                                         |
| Legacy Agent Exceptions                   | Define exception rules for the prevention profile for all endpoints (legacy functionality)                       |
| Support Exception rules                   | Generate exceptions based on files provided by the support team                                                  |

#### All Issue Exception & Exclusion Rules

This rule type covers two distinct kinds of rules:

- **Issue Exception rules** — override or bypass the base security policy at the agent/endpoint level, preventing a process from being blocked, a protection module from acting, or a detection from happening at all.
- **Exclusion rules (Issue Exclusion)** — act at the level of already-generated alerts/issues. The agent still detects the behavior on the endpoint, but Cortex Cloud does not save or display that alert.

The documentation states this clearly:

> "The agent continues to generate excluded issues on the endpoint, but they are not saved or displayed in Cortex Cloud."

**When to use which**

| Use Case                                                                 | Rule to Use     |
|----------------------------------------------------------------------------|------------------|
| Prevent the detection/prevention behavior from triggering at all (e.g. allow an executable to run, disable a prevention module, exclude a folder from scanning) | Issue Exception  |
| Let detection continue on the endpoint, but stop the resulting alert from being saved/shown in Cortex Cloud (e.g. suppress known noisy/false-positive issues) | Exclusion        |

#### Disable Prevention Rules vs. Disable Injection and Prevention Rules

**Disable Prevention rules**

Define granular, standing exceptions to the prevention actions triggered by specific security modules. You match target files/processes by hash (SHA256), file/folder path, command line, signer name, or certificate thumbprint (wildcards supported for command line and paths), then select which modules should skip their prevention actions for that match. The rule applies until removed — there is no time limit.

- **Path**: Settings → Exception Configuration → Disable Prevention Rules
- **Scope**: Global (all endpoints) or specific Exception Profiles
- **Requires**: Cortex XDR agent 7.9+
- Cortex Cloud still generates issues from the disabled prevention actions

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Add-a-disable-prevention-rule-for-endpoints)

**Disable Injection and Prevention rules**

Create a *temporary* exception that bypasses a specific process (matched by process name + path) from both the prevention modules and the injection modules. Useful for short-lived operational needs, such as troubleshooting or running a one-off maintenance process that would otherwise be blocked or injected into.

- **Path**: Settings → Exception Configuration → Disable Injection and Prevention
- **Time limit**: 48 hours by default, configurable up to 1 week — the rule must be explicitly enabled and expires automatically
- **Scope**: Global or specific Exception Profiles
- **Requires**: agent version 7.9+
- Cortex Cloud still generates issues from the bypassed data collections

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Add-a-disable-injection-and-prevention-rule)

**When to use which**

| Use Case                                                                                          | Rule to Use                          |
|-----------------------------------------------------------------------------------------------------|----------------------------------------|
| Permanently exempt a known-good file/process (by hash, path, signer, or certificate) from specific prevention modules | Disable Prevention                   |
| Also need to bypass **injection** modules, not just prevention                                     | Disable Injection and Prevention     |
| The exception is only needed short-term (troubleshooting, maintenance window)                      | Disable Injection and Prevention     |
| The exception should remain in place indefinitely, with fine-grained module selection              | Disable Prevention                   |

#### Support Exception Rules

A special type of exception where you don't define the criteria yourself — instead, you import a JSON file provided by Palo Alto Networks/Cortex Cloud support and apply it as a rule.

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Add-a-support-exception-rule-for-endpoints)

## Cortex XDR Agent

!!! note "Critical Environment (CE)"
    After you install the Cortex XDR agent and the agent registers with Cortex Cloud, you can set endpoints to run with a Cortex XDR agent Critical Environment (CE) version.

    CE versions are designed for sensitive and highly regulated environments. These versions receive full content update coverage and contain the same feature set as the standard line it is based on. Please note that some bug fixes, introducing higher stability risk, may not be incorporated into the maintenance releases of these lines. Support is provided for CE versions for 24 months, while support for standard versions is provided for 9 months.

    [Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Set-a-Cortex-XDR-agent-Critical-Environment-version)

!!! note "Application Proxy"
    Cortex XDR agents support using an application proxy for endpoints without direct internet access.

    [Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Set-an-application-proxy-for-Cortex-XDR-agents)

## Endpoint Groups

If you have the Cloud Identity Engine configured, you can also use Active Directory attributes (user, AD group, computer) to define groups — which is very useful in enterprise Windows environments with Domain Controllers.