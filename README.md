# CST8919 - DevOps Security and Compliance

## Assignment 2: Cloud Service Alternatives Report

**Student Name:** Khalid Amchat  
**Student Number:** 041125350
**Date:** August 5, 2026  

---

## Introduction

This report compares Microsoft Azure services used in CST8919 with their closest Amazon Web Services (AWS) and Google Cloud Platform (GCP) alternatives. The comparison focuses on:

- Overview
- Core features
- Security and compliance
- General pricing model
- DevSecOps integration

Cloud services are not always exact one-to-one equivalents. In several cases, AWS or GCP requires a combination of services to provide functionality similar to one Azure service.

> **Naming note:** Azure Active Directory is now **Microsoft Entra ID**, and Azure Sentinel is now **Microsoft Sentinel**.

> **Compliance note:** Cloud services can support standards such as ISO 27001, SOC, PCI DSS, HIPAA, GDPR, NIST, and CIS. However, using a service does not automatically make an organization compliant. The organization must configure and operate it correctly.

---

## Quick Reference Comparison

| Azure service | Closest AWS alternative | Closest GCP alternative |
|---|---|---|
| Azure Active Directory / Microsoft Entra ID | AWS IAM Identity Center + AWS IAM | Cloud Identity + Google Cloud IAM |
| Azure Monitor | Amazon CloudWatch | Cloud Monitoring |
| Log Analytics | CloudWatch Logs Insights | Cloud Logging + Log Analytics |
| Azure Policy | AWS Organizations SCPs + AWS Config | Organization Policy Service |
| Microsoft Defender for Cloud | Security Hub CSPM + GuardDuty + Inspector | Security Command Center |
| Microsoft Sentinel | Security Lake + OpenSearch Security Analytics + automation services | Google Security Operations |
| Azure Resource Manager | AWS CloudFormation + AWS Resource Groups | Cloud Resource Manager + Infrastructure Manager |
| Azure Logic Apps | AWS Step Functions + EventBridge | Workflows + Eventarc |
| Azure Key Vault | Secrets Manager + KMS + Certificate Manager | Secret Manager + Cloud KMS + Certificate Manager |

---

# 1. Azure Active Directory / Microsoft Entra ID

| Area | Microsoft Azure | AWS | Google Cloud |
|---|---|---|---|
| **Overview** | Microsoft Entra ID manages users, groups, applications, authentication, SSO, and access to Azure and Microsoft 365. | IAM Identity Center manages workforce access and SSO. AWS IAM controls permissions to AWS resources. | Cloud Identity manages users and groups. Google Cloud IAM controls access to cloud resources. |
| **Core features** | SSO, MFA, Conditional Access, RBAC, managed identities, federation, Privileged Identity Management, and identity-risk detection. | Permission sets, IAM policies, roles, temporary credentials, MFA, federation, and multi-account access. | SSO, MFA, federation, predefined and custom roles, service accounts, IAM Conditions, and workload identity federation. |
| **Security and compliance** | Supports least privilege, identity governance, access reviews, sign-in logs, audit logs, and risk-based controls. | Supports least privilege, temporary credentials, CloudTrail auditing, policy analysis, and centralized account access. | Supports least privilege, audit logs, context-aware controls, organization policies, and service-account management. |
| **Pricing** | Basic features are available free. Premium identity and governance features are generally licensed per user. | IAM and IAM Identity Center generally have no additional service charge. Related logging and security services may cost extra. | Cloud Identity has free and Premium editions. Google Cloud IAM has no separate usage charge. |
| **DevSecOps integration** | Azure CLI, PowerShell, Microsoft Graph, Bicep, Terraform, GitHub Actions, and Azure DevOps workload federation. | AWS CLI, SDKs, CloudFormation, Terraform, CodePipeline, and GitHub Actions using OIDC. | `gcloud`, APIs, Terraform, Cloud Build, service accounts, and Workload Identity Federation. |

### Analysis

- Entra ID is the best fit for organizations using Microsoft 365 and Azure.
- AWS separates workforce identity from cloud-resource authorization.
- Google Cloud also separates identity management from resource permissions.
- All three support short-lived identities for secure CI/CD pipelines.

---

# 2. Azure Monitor

| Area | Microsoft Azure Monitor | Amazon CloudWatch | Google Cloud Monitoring |
|---|---|---|---|
| **Overview** | Collects and analyzes metrics, logs, traces, and events from Azure and supported external environments. | Monitors AWS resources and applications using metrics, dashboards, alarms, logs, and events. | Collects metrics and provides dashboards, uptime checks, service monitoring, and alerting. |
| **Core features** | Metrics, alerts, dashboards, workbooks, Application Insights, Prometheus support, and diagnostic settings. | Metrics, alarms, dashboards, Application Signals, custom metrics, and service integrations. | Metrics, dashboards, alerting policies, uptime checks, Prometheus integration, and service-level monitoring. |
| **Security and compliance** | Azure RBAC, encryption, activity logs, diagnostic settings, and configurable data retention. | IAM permissions, CloudTrail auditing, KMS encryption options, and log-retention controls. | IAM, Cloud Audit Logs, data-location controls, encryption, and configurable retention. |
| **Pricing** | Costs may include metric volume, alerts, data ingestion, retention, and Application Insights usage. | Costs may include custom metrics, dashboards, alarms, API requests, and advanced monitoring features. | Costs may include chargeable metrics, monitoring API usage, and advanced observability features. |
| **DevSecOps integration** | Bicep, ARM, Terraform, Azure CLI, APIs, Azure DevOps, GitHub Actions, Functions, and Logic Apps. | CloudFormation, CDK, Terraform, CLI, SDKs, EventBridge, Lambda, and SNS. | Terraform, `gcloud`, APIs, Cloud Build, Pub/Sub, Cloud Functions, and Workflows. |

### Analysis

- Azure Monitor is tightly integrated with Azure resources and Microsoft security tools.
- CloudWatch is the natural monitoring platform for AWS services.
- Cloud Monitoring provides strong dashboards and alerting for GCP and supported external systems.
- Monitoring configurations should be deployed as infrastructure as code.

---

# 3. Log Analytics

| Area | Azure Log Analytics | CloudWatch Logs Insights | Cloud Logging and Log Analytics |
|---|---|---|---|
| **Overview** | A tool within Azure Monitor for querying data stored in Log Analytics workspaces. | An interactive query service for analyzing logs stored in CloudWatch Logs. | Cloud Logging stores and searches logs, while its analytics capabilities support large-scale log analysis. |
| **Core features** | Kusto Query Language, workspaces, saved queries, alerts, workbooks, log-based analysis, and integration with Sentinel. | Interactive queries, pattern analysis, field discovery, dashboards, alarms, and log groups. | Logs Explorer, log buckets, views, routing sinks, SQL-based analytics, log-based metrics, and alerts. |
| **Security and compliance** | RBAC, workspace permissions, encryption, retention controls, private connectivity, and data-export controls. | IAM, KMS integration, log-group retention, encryption, and CloudTrail auditing. | IAM, regional log buckets, retention controls, encryption, audit logs, and VPC Service Controls support. |
| **Pricing** | Mainly based on data ingestion, retention, export, queries, and selected table plans. | Mainly based on log ingestion, storage, scanning during queries, and data delivery. | Mainly based on log ingestion, retention beyond included limits, analytics, and exported data. |
| **DevSecOps integration** | KQL, REST APIs, Azure CLI, PowerShell, Terraform, Bicep, alerts, Functions, Logic Apps, and Sentinel. | AWS CLI, SDKs, CloudFormation, Terraform, subscriptions, Lambda, EventBridge, and alarms. | `gcloud`, APIs, Terraform, Log Router, Pub/Sub, BigQuery, Cloud Functions, and Workflows. |

### Analysis

- Log Analytics is not a separate monitoring platform; it is a major analysis component of Azure Monitor.
- KQL makes it useful for operations, troubleshooting, security investigations, and Microsoft Sentinel.
- CloudWatch Logs Insights is the closest AWS query service.
- GCP combines log collection, routing, storage, and analytics through Cloud Logging.

---

# 4. Azure Policy

| Area | Azure Policy | AWS Organizations SCPs + AWS Config | Google Organization Policy |
|---|---|---|---|
| **Overview** | Enforces organizational standards and evaluates Azure resources for compliance. | SCPs create account-level permission guardrails. AWS Config records configurations and evaluates resource compliance. | Applies centralized constraints across organizations, folders, and projects. |
| **Core features** | Audit, deny, modify, deploy-if-not-exists, initiatives, compliance reports, and remediation tasks. | Preventive SCP guardrails, Config rules, configuration history, conformance packs, and remediation. | Built-in and custom constraints, inheritance, dry-run testing, tags, and hierarchical enforcement. |
| **Security and compliance** | Includes built-in initiatives mapped to common benchmarks and regulatory frameworks. | Config conformance packs and Security Hub controls can support CIS, NIST, PCI DSS, and other frameworks. | Organization policies and Security Command Center can support benchmark and compliance requirements. |
| **Pricing** | Azure Policy for Azure resources has no additional charge. Supporting services may create costs. | SCPs are free. AWS Config is charged based on configuration items and rule evaluations. | Organization Policy API usage has no separate charge. Supporting security services may cost extra. |
| **DevSecOps integration** | Policy as code with JSON, Bicep, ARM, Terraform, CLI, PowerShell, APIs, and CI/CD pipelines. | JSON policies, CloudFormation, Terraform, AWS Config rules, CloudFormation Guard, EventBridge, and Systems Manager. | Terraform, `gcloud`, REST APIs, Cloud Build, custom constraints, Pub/Sub, and automation services. |

### Analysis

- Azure Policy provides preventive, detective, and remediation features in one main service.
- AWS requires both SCPs and AWS Config because they solve different problems.
- GCP Organization Policy is strong for hierarchical preventive controls.
- Policy definitions should be stored in Git and reviewed through pull requests.

---

# 5. Microsoft Defender for Cloud

| Area | Microsoft Defender for Cloud | AWS security services | Google Security Command Center |
|---|---|---|---|
| **Overview** | A cloud-native application protection platform providing security posture management and workload protection. | Security Hub CSPM manages posture, GuardDuty detects threats, and Inspector identifies vulnerabilities. | Centralized cloud-risk, posture, vulnerability, threat, and compliance management. |
| **Core features** | Secure Score, recommendations, attack paths, regulatory dashboard, vulnerability management, and workload protection. | Security standards, findings, threat detection, vulnerability scanning, exposure analysis, and automation. | Findings, attack paths, posture management, threat detection, vulnerability detection, and compliance views. |
| **Security and compliance** | Supports Azure, hybrid, AWS, and GCP environments and maps controls to common standards. | Security Hub standards and Config-backed controls help assess AWS security and compliance. | Monitors against benchmarks such as CIS, NIST, PCI DSS, and other supported frameworks. |
| **Pricing** | Basic posture features and optional paid Defender plans charged by protected resource or workload. | Pricing is distributed across Security Hub, GuardDuty, Inspector, Config, and related services. | Standard, Premium, and other supported tiers; paid options may use subscription or usage-based pricing. |
| **DevSecOps integration** | GitHub, Azure DevOps, Defender XDR, Sentinel, Azure Policy, APIs, Terraform, and workflow automation. | EventBridge, Lambda, Systems Manager, Security Hub APIs, CloudFormation, Terraform, and CI/CD tools. | APIs, Pub/Sub notifications, Terraform, Cloud Build, Workflows, cases, and playbooks in supported tiers. |

### Analysis

- Defender for Cloud provides broad security capabilities through one integrated Microsoft platform.
- AWS offers comparable capabilities through several specialized services.
- Security Command Center is the closest centralized GCP alternative.
- Security findings should be linked to automated ticketing and remediation workflows.

---

# 6. Microsoft Sentinel

| Area | Microsoft Sentinel | AWS-native combination | Google Security Operations |
|---|---|---|---|
| **Overview** | A cloud-native SIEM and SOAR platform for collecting, detecting, investigating, and responding to threats. | Security Lake centralizes normalized security data; OpenSearch Security Analytics provides detections and dashboards; other services provide automation. | A managed SIEM and SOAR platform for security analytics, threat detection, investigation, case management, and playbooks. |
| **Core features** | Data connectors, KQL, analytics rules, incidents, hunting, workbooks, automation rules, and playbooks. | OCSF-normalized data, security findings, detectors, OpenSearch dashboards, Athena queries, and event-driven response. | Data ingestion, normalization, detection rules, threat intelligence, search, cases, and SOAR playbooks. |
| **Security and compliance** | RBAC, encryption, retention controls, audit data, incident evidence, and multicloud connectors. | IAM, S3 security, Lake Formation, KMS, CloudTrail, data lifecycle policies, and Security Hub controls. | IAM, centralized telemetry, retention controls, auditability, and supported compliance programs. |
| **Pricing** | Primarily based on security-data ingestion, analytics tier, retention, and automation usage. | Distributed across Security Lake, S3, OpenSearch, Athena, Security Hub, Lambda, and other services. | Generally based on contracted security-data ingestion, retention, and selected SIEM/SOAR package. |
| **DevSecOps integration** | APIs, KQL, GitHub, Azure DevOps, content as code, Logic Apps, Defender products, and CI/CD deployment. | CloudFormation, CDK, Terraform, APIs, EventBridge, Lambda, Step Functions, and Systems Manager. | APIs, parsers, detection rules, integrations, cases, playbooks, and controlled content deployment. |

### Analysis

- Sentinel and Google Security Operations are the closest direct managed SIEM/SOAR competitors.
- AWS does not have one exact equivalent and normally uses a multi-service architecture.
- Security Lake alone should not be described as a complete replacement for Sentinel.
- SIEM cost depends heavily on ingestion volume and data retention.

---

# 7. Azure Resource Manager

| Area | Azure Resource Manager | AWS CloudFormation + Resource Groups | Cloud Resource Manager + Infrastructure Manager |
|---|---|---|---|
| **Overview** | Azure’s management and control-plane service for deploying, organizing, securing, and managing resources. | CloudFormation provisions resources from templates; Resource Groups organize related resources. | Cloud Resource Manager manages the hierarchy; Infrastructure Manager deploys Terraform-based infrastructure. |
| **Core features** | Resource groups, subscriptions, management groups, tags, locks, RBAC, ARM templates, deployments, and activity logs. | Stacks, templates, change sets, drift detection, StackSets, tags, resource groups, and rollback. | Organizations, folders, projects, IAM inheritance, tags, Terraform deployments, revisions, and previews. |
| **Security and compliance** | Entra authentication, Azure RBAC, Policy, locks, activity logs, managed identities, and deployment history. | IAM, stack policies, CloudTrail, change sets, drift detection, rollback, and CloudFormation Guard. | IAM hierarchy, organization policies, audit logs, Terraform validation, deployment previews, and service accounts. |
| **Pricing** | Resource Manager and ARM template deployment have no separate charge; deployed resources are billed. | CloudFormation generally has no additional charge for standard AWS resources; some extensions may have charges. | Cloud Resource Manager has no separate charge. Infrastructure Manager may include deployment-management costs. |
| **DevSecOps integration** | ARM templates, Bicep, Terraform, Azure CLI, PowerShell, REST APIs, GitHub Actions, and Azure DevOps. | YAML/JSON templates, CDK, Terraform, CLI, CodePipeline, GitHub Actions, change sets, and drift detection. | Terraform, Infrastructure Manager, `gcloud`, APIs, Git repositories, Cloud Build, and deployment previews. |

### Analysis

- Resource Manager is broader than an infrastructure-template engine because it is Azure’s main resource control plane.
- CloudFormation is the closest AWS deployment service.
- GCP requires Cloud Resource Manager for hierarchy and Infrastructure Manager for managed IaC deployment.
- Bicep, CloudFormation, and Terraform configurations should be version-controlled and validated before deployment.

---

# 8. Azure Logic Apps

| Area | Azure Logic Apps | AWS Step Functions + EventBridge | Google Cloud Workflows + Eventarc |
|---|---|---|---|
| **Overview** | A managed low-code service for creating workflows that integrate applications, data, APIs, and cloud services. | Step Functions orchestrates distributed applications; EventBridge routes events between services. | Workflows orchestrates services and APIs; Eventarc routes events to supported destinations. |
| **Core features** | Visual designer, triggers, actions, conditions, loops, connectors, retries, monitoring, and stateful workflows. | State machines, Standard and Express workflows, retries, error handling, service integrations, and event routing. | YAML/JSON workflows, HTTP calls, connectors, conditions, retries, callbacks, service accounts, and event triggers. |
| **Security and compliance** | Entra ID, managed identities, RBAC, private networking options, Key Vault integration, and workflow history. | IAM roles, CloudWatch Logs, CloudTrail, KMS, private endpoints, and service-level permissions. | IAM service accounts, Cloud Logging, Cloud Audit Logs, VPC Service Controls support, and Cloud KMS options. |
| **Pricing** | Consumption plans charge per trigger, action, and connector operation. Standard plans charge for reserved hosting capacity. | Standard workflows charge per state transition; Express workflows are based on executions, duration, and memory. | Generally charged by workflow steps and external service usage; event services may add charges. |
| **DevSecOps integration** | ARM, Bicep, Terraform, VS Code, Azure DevOps, GitHub Actions, APIs, Monitor, and Sentinel playbooks. | CloudFormation, CDK, Terraform, CodePipeline, EventBridge, Lambda, CloudWatch, and APIs. | Terraform, `gcloud`, Cloud Build, Eventarc, Pub/Sub, Cloud Run, Cloud Functions, and APIs. |

### Analysis

- Logic Apps has a large connector ecosystem and is useful for low-code integration and security playbooks.
- Step Functions is stronger as code-oriented AWS service orchestration, while EventBridge handles event routing.
- GCP Workflows is the closest orchestration service, with Eventarc supplying event-driven triggers.
- Workflow identities should use least privilege and secrets should not be hard-coded.

---

# 9. Azure Key Vault

| Area | Azure Key Vault | AWS Secrets Manager + KMS + Certificate Manager | Secret Manager + Cloud KMS + Certificate Manager |
|---|---|---|---|
| **Overview** | Centrally stores and manages secrets, cryptographic keys, and certificates. | Secrets Manager manages secrets, KMS manages encryption keys, and ACM manages supported certificates. | Secret Manager stores secrets, Cloud KMS manages cryptographic keys, and Certificate Manager manages certificates. |
| **Core features** | Secret versioning, keys, certificates, rotation support, HSM options, soft delete, purge protection, and audit logging. | Secret rotation, versioning, KMS keys, key policies, HSM-backed cryptography, certificate provisioning, and audit logs. | Secret versions, rotation workflows, KMS keys, HSM protection, certificate management, replication, and audit logs. |
| **Security and compliance** | Entra ID authentication, Azure RBAC, private endpoints, encryption, Managed HSM, soft delete, and purge protection. | IAM, resource policies, KMS encryption, CloudTrail, private endpoints, rotation, and CloudHSM options. | IAM, customer-managed encryption, audit logs, regional controls, VPC Service Controls, and Cloud HSM. |
| **Pricing** | Based mainly on operations, certificate operations, key type, HSM protection, and premium features. | Secrets Manager charges per stored secret and API calls; KMS charges for keys and cryptographic operations. | Secret Manager charges for versions and access operations; Cloud KMS charges for keys and operations. |
| **DevSecOps integration** | Managed identities, SDKs, REST APIs, CLI, PowerShell, Bicep, Terraform, GitHub Actions, and Azure DevOps. | IAM roles, SDKs, CLI, CloudFormation, Terraform, Lambda rotation, CodePipeline, ECS, EKS, and Lambda. | Service accounts, SDKs, `gcloud`, Terraform, Cloud Build, GKE, Cloud Run, Functions, and rotation automation. |

### Analysis

- Key Vault combines secrets, keys, and certificates in one Azure service family.
- AWS and GCP separate these responsibilities across multiple products.
- Applications should retrieve secrets at runtime through workload identities.
- Secrets must never be committed to Git repositories or stored directly in pipeline files.

---

# Overall Comparison

## Main Strengths

| Provider | Main strengths |
|---|---|
| **Microsoft Azure** | Strong integration among Entra ID, Azure Monitor, Policy, Defender for Cloud, Sentinel, Resource Manager, Logic Apps, and Key Vault. |
| **AWS** | Highly specialized services, detailed IAM controls, flexible event-driven architecture, and strong infrastructure-as-code options. |
| **Google Cloud** | Clear resource hierarchy, strong centralized security analytics, effective log routing, managed Terraform deployment, and multicloud security capabilities. |

## Important Differences

- **Azure** often provides broader capabilities through tightly integrated services.
- **AWS** frequently requires several specialized services to match one Azure product.
- **GCP** separates identity, hierarchy, infrastructure deployment, secrets, and cryptographic keys into focused services.
- Exact service equivalence depends on architecture and use case.
- Pricing comparisons must include related services, not only the main product.

## DevSecOps Best Practices

- Store infrastructure, policies, detections, and workflows in Git.
- Use pull requests and automated validation before deployment.
- Use managed or workload identities instead of permanent credentials.
- Retrieve secrets from a secrets-management service at runtime.
- Deploy monitoring, logs, alerts, and security controls with the application.
- Apply least privilege to users, pipelines, and workloads.
- Use policy as code to prevent non-compliant deployments.
- Automate response carefully and test workflows before production.
- Control log volume, retention, and SIEM ingestion costs.
- Review audit logs and configuration drift regularly.

---

# Conclusion

Azure, AWS, and Google Cloud provide comparable capabilities for identity, monitoring, governance, security, automation, infrastructure management, and secrets protection. However, the services are organized differently.

Azure provides the most integrated experience for organizations already using Microsoft technologies. AWS offers flexible and specialized services, but several products may be required to match one Azure service. Google Cloud provides strong resource hierarchy, observability, security analytics, and managed workflow and infrastructure tools.

There is no single best provider for every organization. The choice should depend on:

- Existing cloud and software environment
- Team skills
- Security and compliance requirements
- Automation and CI/CD tools
- Multicloud needs
- Data residency
- Workload scale
- Logging and security-data volume
- Total cost of the complete architecture

---

# References

## Assignment

- [CST8919 Assignment 2 – Cloud Service Alternatives Report](https://github.com/Igomaa-AC/CST8919_s2026/blob/main/Assignment2.md)

## Microsoft Azure

- [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/)
- [Azure Monitor overview](https://learn.microsoft.com/en-us/azure/azure-monitor/fundamentals/overview)
- [Azure Monitor Logs overview](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/data-platform-logs)
- [Azure Policy overview](https://learn.microsoft.com/en-us/azure/governance/policy/overview)
- [Microsoft Defender for Cloud overview](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)
- [Microsoft Sentinel overview](https://learn.microsoft.com/en-us/azure/sentinel/overview)
- [Azure Resource Manager overview](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview)
- [Azure Logic Apps overview](https://learn.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)
- [Azure Key Vault overview](https://learn.microsoft.com/en-us/azure/key-vault/general/overview)

## Amazon Web Services

- [AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)
- [AWS IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html)
- [AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html)
- [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-are-securityhub-services.html)
- [Amazon Security Lake](https://docs.aws.amazon.com/security-lake/latest/userguide/what-is-security-lake.html)
- [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)
- [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
- [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)

## Google Cloud

- [Cloud Identity](https://docs.cloud.google.com/identity/docs/overview)
- [Google Cloud IAM](https://docs.cloud.google.com/iam/docs/overview)
- [Cloud Monitoring](https://docs.cloud.google.com/monitoring/docs/monitoring-overview)
- [Cloud Logging](https://docs.cloud.google.com/logging/docs/overview)
- [Organization Policy](https://docs.cloud.google.com/organization-policy/overview)
- [Security Command Center](https://docs.cloud.google.com/security-command-center/docs/security-command-center-overview)
- [Google Security Operations](https://cloud.google.com/security/products/security-operations)
- [Infrastructure Manager](https://docs.cloud.google.com/infrastructure-manager/docs/overview)
- [Google Cloud Workflows](https://docs.cloud.google.com/workflows/docs/overview)
- [Secret Manager](https://docs.cloud.google.com/secret-manager/docs/overview)
- [Cloud Key Management Service](https://docs.cloud.google.com/kms/docs)

---
