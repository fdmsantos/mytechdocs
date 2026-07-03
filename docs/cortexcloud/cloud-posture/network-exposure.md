# Network Exposure Detection

## Overview

[Documentation](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Network-exposure-detection)

## Inbound Internet Exposure Detection

### Supported Asset Types

CNA detects internet exposure for the following cloud services and asset types:

| Provider / Service | AWS | Azure | GCP |
|---|---|---|---|
| Managed virtual machines | Amazon EC2 | Azure Virtual Machines | GCP Compute Instances |
| Managed databases | RDS, Redshift | Azure SQL, Azure Database for PostgreSQL, Azure Database for MySQL, Cosmos DB | – |
| Serverless functions | AWS Lambda | – | – |
| Managed Kubernetes | EKS (services behind load balancer and ingress) | AKS (services behind load balancer and ingress) | GKE (services behind load balancer and ingress) |

CNA supports Kubernetes containers exposed to the internet behind a load balancer or behind an ingress.

### Kubernetes Services

CNA detects workloads exposed to the internet in Kubernetes clusters using Kubernetes configuration analysis and external scanning. The workloads must meet these requirements:

- Kubernetes clusters must be onboarded to Cortex Cloud as described in Onboard the Kubernetes Connector.
- Managed Kubernetes offerings in AWS (EKS, ROSA), Azure (AKS, ARO), and GCP (GKE) are supported.
- Supported workloads include ReplicaSet, Deployment, DaemonSet, StatefulSet, and CronJob.

A workload is considered reachable from the internet when the following criteria are met:

- The Kubernetes workload is exposed behind a load balancer or an ingress.
- Kubernetes network policies permit inbound traffic.

## Outbound Internet Exposure Detection

### Supported Asset Types

CNA can detect outbound internet exposure in the following cloud services and asset types:

| Provider / Service | AWS | Azure | GCP |
|---|---|---|---|
| Managed virtual machines | Amazon EC2 | – | – |

## East-West Exposure Detection

### Supported Asset Types

CNA can detect east-west exposure in the following cloud services and asset types:

| Provider / Service | AWS | Azure | GCP |
|---|---|---|---|
| Managed virtual machines | Amazon EC2 | – | – |
