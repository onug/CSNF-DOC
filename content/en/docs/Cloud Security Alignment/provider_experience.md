---
title: "Provider Experience"
linkTitle: "Provider Experience"
weight: 100
description: >-
     CSNF: Provider Experience
---

### Provider Experience

We need to consider certain major provider types:
Cloud Providers (for e.g., Azure, AWS, other clouds)

1. Cloud providers naturally is one of the major sources of the data of interest, and for many types of data cloud providers may be the only source of certain critical data.
   
   It is important to highlight that we should consider cloud providers of different types, covering IaaS, PaaS and SaaS. For e.g., typical enterprise may be hosting some IaaS resources on AWS and Azure, may be using PaaS resources (e.g. DynamoDB or Azure CosmosDB), at the same time also using Office 365 SaaS services for productivity apps, ADP services for paystub and employee taxes handling and PagerDuty to facilitate OnCall and LiveSite alerting. The enterprise would need to collect and process security-significant events for all services it is using.
2. 3rd party monitoring and other services
   
   Cloud customers are often using various 3rd party services to monitor their apps & services. A lot of monitoring is focused on Operational monitoring, but there is also security aspect of it. For e.g., application-level security and audit logs are often collected by the monitoring service.
   
   There are also specialized security services that may be protecting internet-facing endpoints, and/or services that are monitoring internet-facing endpoints and alert on potential security exposures.
   
   These services would be important producers of security events.
3. Security data enrichment and processing services
   
   Some of security monitoring services will also act as data processors – collecting various audit and other data from customer services, and then producing security detections and other events. For e.g., ML models may be used to detect unusual activity, or unexpected admin actions etc.
   
   Some of services of this type are offered by CSPs (e.g., Azure Security Center), others are offered by 3rd parties.
