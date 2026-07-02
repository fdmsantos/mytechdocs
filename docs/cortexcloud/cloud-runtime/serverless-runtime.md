# Serverless Function Runtime Security

## Overview

Cortex Cloud enables runtime monitoring within a cloud environment by embedding Cortex XDR agent directly into the code of the serverless function. This allows for real-time monitoring of code execution, processes, networking, and filesystem activity, along with the enforcement of policies to permit or deny these actions. This in-depth runtime visibility enhances the overall security of your serverless functions.

Policy violations are detected and logged in Cortex Issues to allow for effective scoping and analysis in order to thoroughly assess the issues.

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Serverless-function-runtime-security)

## Supported Platforms

Runtime protection for serverless functions is available for Cortex Cloud Runtime Security, Cortex XSIAM Premium, Cortex XSIAM Enterprise, and Cortex XSIAM NG Siem licenses.

**Supported runtime environments**: Python, Node.js

**Supported architecture**: x86_64

**Supported cloud provider**: Amazon Web Services (AWS)

## Setup

1. [Configure restriction profile for serverless functions](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Set-up-restrictions-prevention-profiles)
2. [Create a new policy rule for serverless functions](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Manage-endpoint-prevention-profiles)
3. [Create a serverless function agent package](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Create-an-agent-installation-package)

## Runtime Issues

Every policy violation creates an issue per type:

| Type | Description |
|------|-------------|
| Process activity | Enables specifying specific allowed list processes, blocking all processes except the main process and detecting crypto mining attempts |
| Network activity | Enables monitoring and enforcement of DNS resolutions, inbound and outbound network connections |
| Filesystem activity | Enables defining specific paths in an allowed or denied list |

!!! note
    Issues triggered within 24 hours, sharing the same name and description, will be aggregated into cases along with issues from the same function per execution.
