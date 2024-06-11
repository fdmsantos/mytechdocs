# Prisma Certified Cloud Security Engineer

[Study Guide](https://www.paloaltonetworks.com/content/dam/pan/en_US/assets/pdf/datasheets/education/pccse-study-guide.pdf)

[BluePrint](https://www.paloaltonetworks.com/content/dam/pan/en_US/assets/pdf/datasheets/education/pccse-blueprint.pdf)

## Domain 1 - Cloud Security Posture Management (CSPM)

### Identify assets in a Cloud account

Asset Inventory dashboard (on the Inventory tab) provides a snapshot of the current state of all cloud resources or asset
* Asset Inventory dashboard has four sections
  * Resource Summary
    * Shows the count of the Total Unique Resources
  * Asset Trend
    * The green (Pass Resources), blue (Total Resources), and red (Fail Resources) trend lines are overlaid to visually display the passed and failed resources against the total resource count.
  * Asset Classification
    * Bar graph for each cloud type (default), region name, account name,
      or service name that depicts the ratio of passed to failed resources for policy checks.
  * Tabular data
    * The table enables you to group the results by account name, cloud region, or
      service name (default)

### Configure Policies

* Custom Policy
  * Can have remediation rules
  * Build Using
    * RQL or Saved search
  * For Cloud Code Security (Queries)
    * Can create configuration policies to scan IAC
      * These policies use JSON instead RQL
* Policy Types
  * Config
    * Monitors resource configurations
    * Two Subtypes
      * Build
        * Enables to check security misconfigurations in IaC Templates and ensure these issues do not make their way into production
      * Run
        * Monitors resources and check for potential issues once these cloud resources are deployed
  * Data
    * Protect against malware and enable data classification. To identify sensitive data in cloud storage buckets, they use machine learning and pattern matching.
  * Network
    * Monitor network activities in your environment
  * Audit Event
    * Monitor audit events in your environment for potential policy  violations. 
    * Create audit policies to flag sensitive events such as root activities or configuration changes that may potentially put your cloud environment at risk
  * IAM
    * IAM policies monitor the identities in your cloud environment for excess or unused permissions
* Remediation
  * To Enabled it, Prisma Cloud requires write access to the cloud platform
  * Available for Config Policies using Queries
  * If cli command fails, the executions stops at that command
  * Can use variables. Example: gcloud -q compute --project=${account} firewall-rules delete ${resourceName}
  * Variables:
    * $account: Account is the Account ID of your account in Prisma Cloud.
    * $azurescope: (Azure only) Allows you to specify the node in the Azure resource hierarchy
    where the resource is deployed.
    * $gcpzoneid: (GCP only) Allows you to specify the zone in the GCP project, folder, or
    organization where the resource is deployed.
    * $region: Region is the name of the cloud region to which the resource belongs.
    * resourcegroup: (Azure only) Allows you to specify the name of the Azure Resource Group
    that triggered the alert.
    * $resourceid: Resource ID is the identification of the resource that triggered the alert.
    * $resourcename: Resource name is the name of the resource that triggered the alert.

### Configure compliance standards

* In a custom compliance standard, you can add requirements and sections
  * Minimum of one requirement and one section can be associated with policies that check for adherence to your standards
  * Can clone existing compliance and edit it
  * This is done in Compliance tab
  * Only visible in compliance-overview after add the first policy
* Reports
  * Can view, download or schedule recurring reports
  * Contains:
    * Resources passed of failed
    * Detailed findings for each section
    * Description of requirements
    * Recommendations for fixing
  * Can use compliance filters to show only the information you are interested in
    * Set report timeframe
    * Specific cloud regions/types
  * Download as PDF
  * Can email the report on schedule reports

### Configure alerting and notifications

* Alert States
  * Can trigger notification to manual or automatic remediation
  * Can send alerts to several channels:
    * Alert mechanism
    * AWS Security Hub
    * Cortex XDR alerts
    * Cortex XSOAR alerts
    * Email alerts
    * Google Cloud Pub/Sub
    * Google Cloud Security Command Center
    * IBM Cloud Security Advisor
    * JIRA Alerts
    * PagerDuty alerts
    * ServiceNow alerts
    * Slack Alerts
    * Splunk alerts
    * Webhook alerts
* Alert Rules
  * (For run-time checks) Enable you to define policy violations in selected set of cloud accounts for which you want to trigger alerts
  * Alert Rule can contains several policies and account groups (Can be granularity => By cloud type, Region, cloud Resource, Tags )
  * Automatically Remediation Alerts is only available for default policies (Config policies only) that are designated as Remediable
  * 3 Additional Options
    * Auto Dismiss - Auto dismiss alert for resources identified by their assigned tags
    * Alert Notifications - To notify to all configured channels (Third Party integrations)
      * Can Delay notification for config alert for specific number of minutes
      * Can configure Notifications for when alert is Open, Dismissed, Snoozed or ignored
    * Auto-remediate - to remediate all remediable alerts
  * If you have internal networks that you want to exclude from being flagged in an alert, you can add Trusted IP Addresses on Prisma Cloud.
* Alerts Reports
  * Cloud Security Assessment Report (PDF)
    * that summarizes the risks from open alerts in the monitored cloud accounts for a specific cloud type
  * Business Unit Report (CSV)
    * Includes the total number of resources that have open alerts against policies for any compliance standard, and you can generate the report on demand or on a recurring schedule
* Alerts Workflow
  * Alert profile – Specifies which events should be sent to which channel. You can create any number of alert profiles, where each profile gives you granular control over which audience should receive which notifications.
  * Alert channel – Messaging medium over which alerts are sent. Prisma Cloud supports
    email, JIRA, Slack, PagerDuty, and others.
  * Alert trigger – Specifies what alerts you want to send to the provider included in the profile. Alerts are generated when the rules included in your policy are violated, and you can choose whether you want to send a notification for the detected issues. For example, on runtime violations, compliance violations, cloud discovery, or WAAS.

### Use third-party integrations

* Inbound Integrations
  * Amazon GuardDuty, AWS Inspector, Qualys, and Tenable
* Outbound Integrations
  * Others
  * You can check status updates on demand in Settings Integrations by clicking the Get Status icon for the relevant integration. T
    * The status check displays red if the integration fails
      validation checks for accessibility or credentials; it displays green when the integration is
      working and all templates are valid


-------------------

# New Study

* Supported Public Cloud
  * AWS, Azure, GCP, Alibaba, and Oracle Cloud
* Roles can be used to define the permission scope of an account group
* To access Prisma Cloud API use Access Key
* Trusted IP address use allow lists so that specific IP addresses do not trigger alerts
* Anomaly Settings use behavioral analytics to prevent attacks and insider threats

## Core Concepts

* Resource
  * A resource in Prisma Cloud is an entity in your public cloud environment. This may be any virtual asset or system user.
  * Cloud resources are acquired by Prisma Cloud after you onboard your public cloud accounts.
* Policy
  * A policy is a statement of acceptable state or behavior.
  * A policy has a type, which indicates the underlying mechanism used to apply the policy.
  * Currently, Prisma Cloud has four types of policies:
    * config
    * audit event
    * network
    * anomaly
  * Predefined policies
    * Adheres to established security best practices such as PCI, GDPR, HIPAA, and NIST. Predefined policies cannot be modified.
  * Custom Policies
    * Create custom policies to monitor for violations and enforce organizational standards.
* Alert Rule
  * An alert rule is a collection of one or more account groups and one or more policies that make up the acceptable use of the public cloud environment.
  * Default Alert Rule
    * Must associated public accounts with default alert rule
* Alert
  * An alert is asserted when a resource is in violation of a policy as defined in an alert rule.
  * There are four alert states:
    * open
    * resolved
    * dismissed
    * snoozed
  * Anomaly Alerts
    * Are not based on RQL but are based on machine learning
    * Cannot be cloned or modified directly

## Compliance Dashboard

* Health and Compliance
 * Provides information related to the compliance posture across various compliance standards and only displays the results for monitored resources that match the policies included within a compliance standard.
* Can create custom compliance standards
  * Create Compliance Standard
  * Add Compliance Requirement
    * Add Compliance Sections (Example: Having MFA)
      * Assign Policies

## Third Party Integrations

* Inbound Integrations
  * Amazon GuardDuty, AWS Inspector, Qualys, and Tenable
* Outbound Integrations
  * Others
  * You can check status updates on demand in Settings Integrations by clicking the Get Status icon for the relevant integration. T
    * The status check displays red if the integration fails
      validation checks for accessibility or credentials; it displays green when the integration is
      working and all templates are valid

## Reports

* Can be customized
  * Cloud Type
  * Account Groups
  * Report Type or Compliance Standard
  * Regions
  * Cloud Accounts

## Onboarding

* SSO
  * Must be IDP initated
  * Only one IDP for each tenant

## Dashboards

* Command Center
  * Incidents
    * Alerts generated by anomalies, audit events or network events
  * Misconfigurations
    * Alerts generated by config policies
  * Exposures
    * Alerts generated by network policies with subtype config
  * Identity Risks
    * Violation on IAM
  * Data Risks
    * Data Policy
* SecOps
  * Asses connect to internet
* NetSecOps
* Data

## Policies

* Config
  * Monitor your resource configurations for potential policy violations
  * Remediables
* Network
  * Monitor network activities in your environment
* Data
  * Protect against malware and enable data classification. These types of policies use machine learning and pattern matching
* Audit
  * Monitor audit events in your environment for potential policy violations
* IAM
  * Enforce user access using the principle of least privilege
* Anomaly
  * Help with identifying unusual user activity
  * UEBA Policies
    * Account hijacking attempts alerting for potential hijacking attempts.
    * Excessive login failures alerting for potential hijacking attempts.
    * Unusual user activity alerting for potential insider threats and account compromise.

## Alerts

* Resource-Rule Pair Concept
  * An alert generates an Alert ID that includes the association between a specific resource and a specific policy. This constitutes the resource-rule pair concept. The resource-rule pairing will trigger the alert only one time.
* Reports can be downloaded in CSV

## Troubleshooting

* AWS
  * Assets Missing or NON Gren Status Indicatores
    * Did you add a CloudWatch log group for Prisma Cloud to capture log streams?
    * Did you enable VPC flow logs on your AWS account?
    * Did you enable other integrations, such as Amazon GuardDuty or AWS Inspector on your AWS account?
    * Did you add an IAM role to publish flow logs to the CloudWatch log group?
    * Does your Prisma Cloud role have the necessary permissions?
    * Did you add an IAM role to publish flow logs to the CLoudWatch log group?
    * Does your CloudWatch IAM role have the necessary permissions?
  * AWS Account or Organization Onboarding
    * What is your AWS External ID? 
    * What is your AWS Role ARN?
  * Data Security Enablement
    * Security scans for malware and classification-sensitive data in your S3 buckets. If you enabled Monitor and Protect mode, Data Security policies will not support auto-remediation.
  * SNS Topic ARN Configuration (used to notify changes to storage objects)
    * You should configure CloudTrail to send notifications to SNS topics.

* Azure
  * Common Issues
    * Unable to read the metadata for Storage Account and Key Vault
    * Unable to read network data
    * Prisma Cloud application does not have required permissions
    * Wrong Service Principal ID provided
    * PowerShell issues
  * Storage Account Issues
    * Reader and Data Access (Reader role with Data Access permission) at the subscription level
      * Applicable only for Storage Accounts (listKeys, listAccounts, Read)
      * All future Storage Accounts in these subscriptions will inherit the permission
  * Role-Based Issues
    * A user that has a Reader role on a subscription can view the Storage Account:
      * By default unable to view the underlying data
      * Find more information by searching "Roles-Based Access Controls for Azure" in the Microsoft documentation

* CGP
  * Roles Necessary
    * Viewer
    * Compute Security Admin
      * For auto-remediation
      * Optional
    * Organization Role Viewer
      * This role is required to onboard a GCP Organization
    * Dataflow Admin
      * Optional
      * Only necessary for dataflow log compression using the dataflow service
    * Folder Viewer
      * Optional
      * Only required if you are onboarding folders in GCP resource hierarchy
    * Prisma Cloud Viewer
      * Custom Role
      * Read Storage bucket metadata (storage.buckets.get)
      * Update bucket IAM policies (storage.buckets.getiampolicy)
  

## Remediation

* RQL
  * Types
    * Config
      * Config queries provide a deeper understanding of resource configurations and vulnerabilities in the cloud environment.
      * Options
        * config from iam where
        * config from network where
        * config from cloud.resource where
    * Event
      * Event queries can be used to search and audit console and API access events in the cloud environment.
    * Network
      * Network queries can be used to monitor network traffic to and from your assets deployed in the cloud environment, and you can then use this data to find previously unidentified network security risks
* Types Remediation
  * Manual
  * Automatic
  * Guided
* Permissions
  * AWS - Write Permissions IAM Role
  * Azure - Storage Account Contributor role at subscription level
  * GCP - Compute Security Admin role
  * Alibaba - System Policy
  * Oracle - Admin access on OCI tenant
* Remediation Variables
  * $resourceid
  * $resourcename
  * $account
  * $region
