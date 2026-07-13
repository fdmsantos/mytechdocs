# Data Management

## Broker VM

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Broker-VM)

The Broker VM is a virtual machine deployed in your environment that creates a secure connection between your network and Cortex Cloud. It is used for routing endpoints, collecting logs, and forwarding logs and files for analysis. It can also be deployed within a high availability (HA) cluster setup.

### When to use it

Use the Broker VM when you need to bridge your on-premises or private network with Cortex Cloud, typically to:

- **Route endpoints** through a single secure connection instead of exposing each endpoint directly.
- **Collect logs** from local sources (syslog, network devices, applications) and forward them to Cortex Cloud.
- **Forward files** from your environment for analysis.
- **Ensure resilience** by running it in an HA cluster so data collection continues if one node fails.

## DataSet Management

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Dataset-management)

### Dataset types

The type of dataset is based on the method used to upload the data. The possible types include:

- **Correlation** — A dataset containing data saved from a correlation rule.
- **Lookup** — A dataset containing key-value pairs that can be used as a reference to correlate to events (for example, a user list with corresponding access privileges). You can import or create a lookup dataset, then reference the values for a certain key, run queries, and take action.
- **Raw** — Every dataset where PANW data is ingested out-of-the-box or third-party data is ingested using a configured dedicated collector. The schema is automatically generated based on the log data collected and the data format sent, such as JSON, CEF, and LEEF.
- **Snapshot** — A dataset that contains only the last successful snapshot of the data, such as Workday or ServiceNow CMDB tables.
- **System** — Cortex Cloud datasets that are created out-of-the-box.
- **User** — If saved by a query using the `target` command, the type can be either User or Lookup.
