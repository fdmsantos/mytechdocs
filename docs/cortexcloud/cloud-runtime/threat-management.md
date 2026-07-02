# Threat Management

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Threat-management)

## Detection Rules

Cortex Cloud uses rules to detect threats in your network and generate issues. There are 3 types of detection rules:

| Rule Type                                   | Description                                                                                                                                                                                                     |
|----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Indicators of Compromise (IOCs)               | Alert for known artifacts considered malicious or suspicious. Static and simple, based on criteria such as SHA256 hashes, IP addresses and domains, file names, and paths. Created from threat-intelligence feeds or investigations within Cortex Cloud (e.g. a known ransomware file hash). |
| Behavioral Indicators of Compromise (BIOCs)   | Detect suspicious behavior (network, process, file, registry, etc.) rather than static artifacts. Alert when a defined behavior pattern is detected. If Cortex Cloud Analytics is enabled, Analytics BIOCs (ABIOCs) establish a baseline of normal behavior and detect deviations from it. |
| Correlation Rules                             | Analyze the relationship between multiple events from multiple sources, using an XQL (Cortex Query Language) based engine.                                                                                    |

## BIOC Rules

| BIOC Type            | Mode                    | Description                                                                                     |
|-----------------------|--------------------------|---------------------------------------------------------------------------------------------------|
| Process               | Execution, Injection     | Search on process execution and injection by name, hash and more                                  |
| File                  | All, Create, Read, Write, Delete, Rename | Search on file create, write, read, delete and rename by file name and path      |
| Network               | All, Incoming, Outgoing, Failed, Raw_Packet | Search on network outgoing, incoming by IP address, port and more                     |
| Image Load            | –                        | Search on module load into process events by module ids and more                                  |
| Registry              | All, Create_Registry_Key, Delete_Registry_Key, Rename_Registry_Key, Set_Registry_Value, Delete_Registry_Value | Search on registry write, rename and delete by value path and data      |
| Event Log             | –                        | Search on event log data, including Windows and Linux event logs, by description, event ID and more |
| Network Connections   | –                        | Search on network connections by session attributes or participants attributes                    |

!!! note "Analytics BIOC (ABIOC)"
    To use Analytics BIOC rules (ABIOCs), Cortex Cloud Analytics must be enabled — it is a mandatory prerequisite. Without it, ABIOCs simply do not exist in the tenant.

    **Path**: Settings → Cortex - Analytics

## Correlation Rules

A Correlation Rule analyzes the relationship between multiple events from multiple sources over time, using the Cortex Query Language (XQL) as the query engine.

While an IOC detects an isolated artifact and a BIOC detects a single point-in-time behavior, a Correlation Rule can answer questions like: *"did two or more suspicious events happen together within a time window?"*

When the query criteria are met within the configured time window, Cortex Cloud automatically generates an issue.

**Example use cases**

- A user has several failed logins followed by a successful login within a short time window → possible brute-force attack followed by access
- A device on a watchlist shows unusual activity
- A device connects to an IP that is on a watchlist
- Two specific events occur within the same 10-minute interval