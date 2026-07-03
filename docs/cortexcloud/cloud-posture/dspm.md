# Data Security Posture Management (DSPM)

## Overview

DSPM is included in the **Cloud Posture Management** license.

[Data Security Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Cortex-Cloud-Data-Security)

[Data Classification Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Cortex-Cloud-Data-Classification)

## Supported

### AWS

- **Storage**: Amazon Simple Storage Service (S3), Amazon EBS volumes
- **Databases**: Amazon Aurora (provisioned), RDS (MySQL, MariaDB Server, PostgreSQL, Amazon RDS instance and cluster snapshots), Amazon DynamoDB, Amazon Redshift
- **Self-Managed Databases**:
    - MongoDB
    - MySQL
    - SQL Server
    - PostgreSQL
    - MariaDB Server

### Azure

- **Storage**: Azure Blob Storage, Azure Managed Disks
- **Databases**: Azure SQL, Azure Cosmos DB, Azure SQL Managed Instance
- **Self-Managed Databases** (outpost scan only):
    - MongoDB
    - MySQL
    - SQL Server
    - PostgreSQL
    - MariaDB

!!! note
    CMK (customer-managed key) in SQL Server is only supported in outpost mode.

### GCP

- **Storage**: Cloud Storage, Persistent Disks
- **Databases**: Cloud SQL (MySQL, PostgreSQL, SQL Server), Bigtable
- **Analytics**: BigQuery
- **Self-Managed Databases**:
    - MongoDB
    - MySQL
    - SQL Server
    - PostgreSQL
    - MariaDB

### OCI (Oracle Cloud Infrastructure)

- **Storage**: OCI Object Storage

### Snowflake

- Account
- Stage
- Database

### Microsoft Office 365

- Tenant
- Microsoft SharePoint site
- Drive
- Document library

### Databricks

- Account
- Workspace
- Metastore
- Catalog (Database)

### On-Premises

- **Databases**:
    - MySQL
    - PostgreSQL
- **File Shares**:
    - SMB
    - NFS

## Review Errors in Cortex Cloud Data Security

1. In the lower left part of the screen, click **Settings → Data Sources**.
2. On the Data Sources screen, click a cloud service provider or other data source and then click the **View Details** link. Alternatively, you can click the **Error** button near the top of the screen. This button displays the data sources that have errors.
3. In the Cloud Instances screen, click the **Instance Name** link. A screen displaying the instance name and related information opens.
4. In the Security Capabilities list, click **Data Security Scanning** to display the Errors pane with the details of the errors that were found.
5. Click **Add Filters** to create a filter that displays the assets you want to review.
6. Click an asset row to display a popup window with information about the error and a raw error string that you can copy.

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/How-to-review-errors-in-Cortex-Cloud-Data-Security)

## Configure the Scanning Settings for Supported Services

1. In the lower left area, click **Settings → Data Sources & Integrations**.
2. On the Data Sources & Integrations page, click a cloud service provider or other data source and then click the **View Details** link.
3. On the Cloud Instances screen, click an instance name link. A screen displaying the instance name opens.
4. At the bottom of the screen, under **Accounts** (AWS), **Subscriptions** (Azure), or **Projects** (GCP), right-click an item in the list and then in the context menu, select **Edit**.

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/How-to-configure-the-scanning-settings-for-supported-services?tocId=~gjn_X8W8GYramEGYnebiQ)