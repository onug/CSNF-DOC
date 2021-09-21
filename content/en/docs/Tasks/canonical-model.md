---
title: "Canonical Data Model"
date: 2021-07-20
update: 2021-08-19
weight: 2
description: Canonical Data Model
---

{{% pageinfo %}}
## **csnf.common.types**
{{% /pageinfo %}}


### Overview

**Ed: Do we need this Overview section on this page - or is the earlier Overview page sufficient?**

What is the Cloud Security Notification Framework (CSNF)? CSNF is an Open Source initiative tackling the difficulty of providing security assurance for Cloud at scale caused by the large volume of events and security state messaging. The problem is compounded when using multiple Cloud Service Providers (CSP’s) due to the the lack of standardized events and alerts amongst CSP’s.  

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
| Guid          | .event |  Identifier |
| Time          | .event | Time - The Timestamp type represents date and time information using ISO 8601 format and is always in UTC time. For example, midnight UTC on Jan 1, 2014 is 2014-01-01T00:00:00Z.  |
| Region        | .event | (optional) Geolocation includes country, state, city, zip and latitude, longitude information |
| Severity      | .event | Event severity set by the provider  |
| State         | .event | (optional) Event state whether active or resolved |
| URL           | .event | Direct URL link to the event for details |
| Name          | .event | Name of event |
| Short Description | .event | (optional) Brief description of event  |
| Long Description | .event | (optional) Detailed description of event  |
| Additional Properties | .event | (optional) Key-Value pairs or property bag for additional missing but critical event properties |
| Event name | .event | Name of event |
|Event identifier |.event | Identifier of the event |
|Event start time |.event | Time at which the event ocurred. The Timestamp type represents date and time information using ISO 8601 format and is always in UTC time. For example, midnight UTC on Jan 1, 2014 is 2014-01-01T00:00:00Z. |
|Event end time|.event| (optional)  Time at which the event ended. The Timestamp type represents date and time information using ISO 8601 format and is always in UTC time. For example, midnight UTC on Jan 1, 2014 is 2014-01-01T00:00:00Z. |
|Event type |.event | Enumeration of Event type - for e.g., Threat, WAF, etc. for security namespace |

### Resource

A resource types describes the resources that the finding refers to.


|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|Guid |.resource | (optional) Resource identifier |
|Type |.resource | (optional) Resource type |
|Name |.resource | (optional) Resource name  |
|Region |.resource | (optional) Resource geolocation includes country, state, city, zip and latitude, longitude information |
|Zone |.resource | (optional) Resource zone |
|Criticality |.resource | (optional) If the resource is critcial or not |
|Severity |.resource | (optional) Resource severity |
|Additional Properties |.resource | (optional) Key-Value pairs or property bag for additional missing but critical event properties  |

### Service

The service type describes the service for or from which the data was collected. The cloud service name is intended to distinguish services running on different platforms within a provider

|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|Guid |.service | (optional) Service identifier |
|Type |.service | (optional) Service type |
|Name |.service | (optional) Service name |
|Region |.service | (optional) Service geolocation includes country, state, city, zip and latitude, longitude information |
|Zone |.service | (optional) Service zone |
|Additional Properties |.service | (optional) Key-Value pairs or property bag for additional missing but critical event properties |

### Threat Actor

The threat actor is the entity that initiated the event


|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|Guid|.threatactor| (optional) Threat actor identifier |
|Type|.threatactor| (optional) Threat actor type |
|Name|.threatactor| (optional) Threat actor name |
|Additional Properties|.threatactor| (optional) Key-Value pairs or property bag for additional missing but critical event properties |

### Decorator

The CSNF decorator provides context awareness for security events in terms of risk, threat, compliance, asset value. The Cloud consumer can also apply a custom decorator to the event based on their own unique business context, for example a custom decoration based on asset value based on criticality, cost or sensitivity could be applied to the event in order to better contextualize and prioritize response based on business context.

The decorator type allows additional context to be added to an individual event from a trusted source without mutating the base event.  There can be multiple decorators applied to the base event. The combination of standardized security events along with correlated knowledge and analytics processing provide new capabilities for the organizations security team to develop and apply fine-grained secuirty policy based on contextual awareness that was previously unknown.


|  Element   |  Namespace  | Description |
| ------------- | ---------- | ------------- |
|References|.decorator| (optional) The source of enrichment information |
|Compliance|.decorator| (optional) Compliance status of enrichment source? |
| Risk                  | .decorator |   (optional) Risk of the event                                                         |
| Data Classification   | .decorator |   (optional) Event classification                                                     |
| Behavior              | .decorator |   (optional) Behavior of entity associated with the event                                                         |
| Vulnerability         | .decorator |   (optional) Vulnerability information pertaining to the event                                                         |
| Threat                | .decorator |   (optional) Threat information pertaining to the event                                                        |
| Custom1 | .decorator |  (optional)  Additional custom information pertaining to the event                                                         |
| Custom2 | .decorator | (optional) Additional custom information pertaining to the event  |

