# Prisma Certified Cloud Security Engineer

[Study Guide](https://www.paloaltonetworks.com/content/dam/pan/en_US/assets/pdf/datasheets/education/pccse-study-guide.pdf)

[BluePrint](https://www.paloaltonetworks.com/content/dam/pan/en_US/assets/pdf/datasheets/education/pccse-blueprint.pdf)


## Domain 3 - Install, Upgrade, and Backup

### Anomaly Settings

* Support thresholds 
  * Unusual Entity Behavior Analysis (UEBA) => Corresponds audit events
  * Unusual Network Activity => corresponds to policies that analyze network-flow logs
*  Training Model Threshold
  * For account hijacking attempts:
    * Low: The behavioral models are based on observing at least 10 events over seven
      days.
    * Medium: The behavioral models are based on observing at least 25 events over 15
      days.
    * High: The behavioral models are based on observing at least 50 events over 30 days.
  * For unusual user activity:
    * Low: The behavioral models are based on observing at least 25 events over seven
    days.
    * Medium: The behavioral models are based on observing at least 100 events over 30
    days.
    * High: The behavioral models are based on observing at least 300 events over 90 days.
* Alert Disposition - is your preference for when you want to be notified of an alert, based on
  the severity of the issue —low, medium, high
  * ● Conservative:
    For unusual user activity—Report on unknown location and service to classify an
    anomaly.
    For account hijacking—Reports on location and activity to log in under travel
    conditions that are not possible, such as logging in from India and the US within
    eight hours.
    For anomalous compute-provisioning activity—Reports on high-severity alerts only
    when an unusual number of instances are created within a short time interval,
    impossible time travel, and belonging to a TOR anonymity network.
    ● Moderate:
    For unusual user activity—Report on unknown location, or both unknown location
    and service to classify an anomaly.
    For anomalous compute-provisioning activity—Reports on medium and higher
    severity alerts.
    ● Aggressive:
    For unusual user activity—Report on either unknown location or service, or both to
    classify an anomaly.
    For account hijacking—Report on unknown browser and operating system,
    impossible time travel, or both.
    For anomalous compute-provisioning activity—Reports on low and higher severity
    alerts.