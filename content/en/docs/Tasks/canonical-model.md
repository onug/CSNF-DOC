---
title: "Canonical Data Model"
date: 2021-07-20
weight: 2
description: Canonical Data Model
---

{{% pageinfo %}}
## **csnf.common.types**
{{% /pageinfo %}}


### Overview

What is the Cloud Security Notification Framework (CSNF)? CSNF is an Open Source initiative tackling the difficulty of provding security assurance for Cloud at scale caused by the large volume of events and security state messaging. The problem is compounded when using multiple Cloud Service Providers (CSP’s) due to the the lack of standardized events and alerts amongst CSP’s.  

CSNF defines a common vocabulary and decorator pattern that can be used to reduce toil, drive consistency and allow enterprises to apply a context-aware approach to security by corellating and acting upon security events across multiple providers at scale. 

### CSNF Namespaces

The CSNF namespaces are used to help categorize and standardize events across multiple providers. The namespace  provides a high level categorization for event data, so that customers can better analyze, visualize, and correlate the data represented in their events.

CSNF improvements are released following [Semantic Versioning](https://semver.org/).

### Provider

Fields related to the cloud (Iaas, SaaS or PasS) infrastructure the events are coming from.

|  Element      |  Namespace  | Description |
| ---------- | ------------- | ------------- |
| Guid          | .provider | Identifier |
| Type          | .provider | IaaS, SaaS or PaaS |
| Name          | .provider | The cloud account name or alias used to identify different entities in a multi-tenant environment. |
| Account_id | .provider | The cloud account or organization id used to identify different entities in a multi-tenant environment. |

### Event

Any observable occurrence in the operations of an information technology service is an event. 

Note: A security event is an observable occurrence that could affect your security posture. Each organization has  its own threshold for designating an event as a security event


|  Element      |  Namespace  | Description |
| ------------- | ----------  | ------------- |
| Guid          | .event |  |
| Time          | .event |  |
| Region        | .event |  |
| Severity      | .event |  |
| State         | .event |  |
| URL           | .event |  |
| Name          | .event |  |
| Short Description | .event |  |
| Long Description | .event |  |
| Additional Properties | .event |  |
| Event name | .event | Name of event |
|Event identifier |.event | id of the event |
|Event start time |.event | time when event occurred |
|Event end time|.event| (optional)  time when event ended |
|Event type |.event | enum of type - for e.g. Threat, WAF, etc. for security namespace |

### Resource

A resource types describes the resources that the finding refers to.


|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|Guid |.resource | (optional) |
|Type |.resource |  |
|Name |.resource |  |
|Region |.resource |  |
|Zone |.resource |  |
|Criticality |.resource | |
|Severity |.resource | |
|Additional Properties |.resource |  |

### Service

The service type describes the service for or from which the data was collected. The cloud service name is intended to distinguish services running on different platforms within a provider

|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|Guid |.service | (optional) |
|Type |.service |  |
|Name |.service |  |
|Region |.service |  |
|Zone |.service |  |
|Additional Properties |.service |  |

### Threat Actor

The threat actor is the entity that initiated the event


|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|Guid|.threatactor|  |
|Type|.threatactor|  |
|Name|.threatactor|  |
|Additional Properties|.threatactor|  |

### Decorator

The CSNF decorator provides context awareness for security events in terms of risk, threat, compliance, asset value. The Cloud consumer can also apply a custom decorator to the event based on their own unique business context, for example a custom decoration based on asset value based on criticality, cost or sensitivity could be applied to the event in order to better contextualize and prioritize response based on business context.

The decorator type allows additional context to be added to an individual event from a trusted source without mutating the base event.  There can be multiple decorators applied to the base event. The combination of standardized security events along with correlated knowledge and analytics processing provide new capabilities for the organizations security team to develop and apply fine-grained secuirty policy based on contextual awareness that was previously unknown.


|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|References|.decorator|  |
|Compliance|.decorator|  |
| Risk                  | .decorator |                                                              |
| Data Classification   | .decorator |                                                              |
| Behavior              | .decorator |                                                              |
| Vulnerability         | .decorator |                                                              |
| Threat                | .decorator |                                                              |
| Custom1 | .decorator |                                                              |
| Custom2 | .decorator | |

