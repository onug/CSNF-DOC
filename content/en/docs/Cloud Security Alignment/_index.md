---
title: "Cloud Security Alignment Document (Change to CSNF Specification?)"
linkTitle: "Cloud Security Alignment Document"
weight: 4
description: >
  The following document is a work-in-progress by the ONUG CSNF Working Group. It lays out the Cloud Security Notification Framework, our Canonical Dictionary, and explains our "Decorator" concept. These sections may be abstracted out into separate documents as we go forward with the project.
---
{{% pageinfo %}}
IMPORTANT:  
At this juncture, edits to this doc are restricted to Working Group Members Preeti Krishna, Peter Campbell, Jenn Slawson, and Michael Thomas Clark.
{{% /pageinfo %}}
___

The [ONUG](https://www.onug.net/) Automated Cloud Governance Working Group has started on a journey to make it easier to consume cloud data shared by different providers in the industry from a security perspective. Refer to the "ONUG Collaborative Problem Statement and Desired Outcome" (Sep 30, '20) document for the problem statement and ONUG goals.

This document provides context on the key types of data of interest, expected typical data providers' and consumers experiences.

### Scope

- The focus of initial iterations is on *data in the Security space*. Other domains (e.g., operational monitoring) may be addressed in the future.
- Need to think of cloud services from the context of IaaS, PaaS, and SaaS and focus on data from Cloud Service Providers (CSPs).
- CSPs include not only Cloud Providers like Azure, Amazon, Google, but also security solutions / services from Symantec, Microsoft Defender for ATP, etc.
- The initial effort is scoped to documenting the common taxonomy.
- Format of the taxonomy is scoped to json or others (Syslog, CEF)? [Where do we get the broadest market adoption- can we be format
  agnostic?\]

### Out of Scope

- The specific technology as to how do these CSPs protect security perimeter.
- Security Solutions not protecting cloud workloads.
- Updating CSPs to include the common taxonomy is not in scope. \[CSPs can update at their own pace as needed at least foundational elements\]
- *Potential future scope* –Tooling for compliance certification to the published taxonomy

### Approach

Each CSP today has capabilities to expose security-significant information and events to consumers. A number of 3`<sup>`rd`</sup>`  party security and non-security services expose and consume this kind of information as well. However, there is no common format or standard for these events,and when consumers of these events need to receive and process eventsfrom multiple sources (for e.g., multiple CSPs or security solutions) – it becomes challenging.

Unfortunately, it is not possible nor practical to ask all CSPs and other security service providers to change their formats to align to common format and schema – each CSP and other security service providers has its own ecosystem and major format change will be highly disruptive for whole ecosystem, also it will make adoption extremely
hard and slow.

In the view of that, ONUG working group decided to pursue "decorator" model - Cloud Security Notification Framework (CSNF), of adding certain key attributes to existing events, helping to map events into common model while allowing event providers to continue to use existing proprietary schemas. Drawback of this approach is added overhead of duplication of certain data elements, particularly when the CSP / solution provider has those fields defined in their schema already. These fields will be included twice, once in their original format and another in the envelope that'll be defined.

### Use Cases

Security-significant information in scope for this work falls into few major types and use cases:

1. **Security detection events**
   
   Provider: These events are produced by CSP and are flagging possible attacks or other abnormal activity that may indicate or may be related to possible security breach. For e.g., antivirus running on IaaS node may detect malicious code, or network-level security monitoring systemmay detect network-level DDoS attack.
   
   Primary consumer of these events: SOC and security monitoring systems.
   
   Key common information for this type of events:
   
   - Time stamp
   - Subscription(s) and Resource(s) information
   - Possible MITRE mappings \[applies to detection but not controls\]
2. **Security audit events**
   
   Provider: These events are produced by CSP, by app itself and/or by 3rd party services to capture security-significant events that don't necessarily indicate an attack but may be required to understand key activities in various systems. Capturing these events is essential for investigations and incident response, for understanding of systems security posture, and for creating audit trail required for compliance and other purposes.
   
   Primary consumer(s) of these events:
   
   - SOC
   - Security team (for security state monitoring, for threat hunting etc.)
   - Compliance team (to demonstrate compliance with various
     certifications)
   
   Key common information for this type of events:
   
   - Time stamp
   - Subscription(s) and Resource(s) information
   - Possible NIST 800-53 mappings
   -
3. **Security State information**
   
   Provider: Security State information describes essential security policies and configurations of the system. For e.g., which ports are open to the internet, whether AV is running on specific node(s), system lockdown applied to IaaS VM(s) etc. These are produced by security solutions that track security posture and manage configurations and policies.
   
   Primary consumer(s) of these events:
   
   - SOC (to enable incident response and analysis)
   - For compliance and for other purposes.

Security State information by itself is not in the form of events – it is more of a set of configurations and policies, however _changes_ of security state information may generate events – for e.g., audit events, and/or detection events. For example, if an operator changed security state using LiveSite tools or via deployment, it will generate audit events, and some of these changes may generate security detection events if new configuration is causing concerns (for e.g. AV disabled on the node).
[ what controls would apply to classify security state - scope controls? - outcome is policies generating events- would this overlap with above buckets?]
[recognize whether the alert is the outcome from a risk / security posture change is important - somehow can we reflect this in this envelope - map state changes like change in threat protection etc.]

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

### Consumer Experience

We need to consider certain major user types that consumes data from the CSPs for different applications.

1. SIEM and SOAR
   
   SIEM is one of the primary consumers of security events of all types, to facilitate SOC, Incident Response and other activities.
   
   For SIEM Security and Audit events are most important, also reference data (e.g., inventory) as well as security state information (policies in place etc.) are essential.
   
   SIEMs are typically offered as independent 3rd party services (e.g., Splunk), some of the SIEMs are also offered by CSPs (e.g., Azure Sentinel).
2. Threat Protection Services
   
   Endpoint protection, advanced monitoring and response capabilities, e.g., Microsoft Defender protection for endpoint, Crowdstrike.
3. Security State Assessment services/products
   
   Security State Assessment allows service owners to keep an eye on security state of their services, including security state of various cloud resources used as a part of (or by) the service.
   
   For Security State Assessment services Security State information and events indicating changes of that information are most important, although many of security products are combining Security State Assessment with Security Response or other functionality, expanding types of data required.
   
   Typically, CSPs these days are offering services that provide Security State Assessment (e.g., Azure Security Center), however there are also number of 3rd party services that are providing Security State and other services (e.g., cybersecurity.att.com).
4. Compliance Processes and Products
   
   Meeting various certifications and compliance requirements typically requires demonstrating that the service has adequate security monitoring & response, security policies/best practices and other capabilities. This assessment usually is based on collecting various information about the service(s) and how these services are being used and run.
   
   Information in scope of this effort will be important and useful for Compliance processes and products, especially NIST 800-53 mappings.

### Proposed experience options TABLE STILL WIP (Jenn)

| Options                    | Pros            |Cons        |
| :-------------------------|:---------------|:----------|
| 1 - Providers include envelope and handle mapping to taxonomy for consuming apps and customers to leverage and work easily with consistent data across multiple CSPs.    |Providers understand... | Inconsistent... |
| 2 – Consumers and apps / SIEMs handle mapping to taxonomy. Work with providers to land this. | Providers do not have to do implementation.       |   Each consumer has to do the same mapping so not optimal. |
| 3 – ONUG delivers public transformation API that supports security data from major CSPs – can be leveraged by consumers and  | Providers do not have to implement.      |    Consumers still need to invest in development to consume data through the APIs. |

Providers have a stake always in all options - Relationships of the mappings are stored in a way by the service providers

Time, cost and risk implications depending on approach of tooling

### Alerts, Controls and Risks

Usually, cloud platforms create events, those can be cloud resources usage events or alerts of all sorts. Many security vendors today attach MITRE tactics tags to their alerts, since an alert usually reflects a probable attack, or attack action, that can be classified to MITRE tactic.

Other security solutions, that deal with compliance (to a set of controls, such as regulation or standard) and report on vulnerabilities or control deficiencies. The connection between a control and an attack technique is not always clear. A control is aimed at mitigating a risk, and sometimes a vulnerability, but an attack can exploit many vulnerabilities, so the lack of a specific control can lead to many potential attacks, but this is dependent in the specific implementation of technology (which is not always part of the equation).

Following that -- if we're dealing with taxonomy the deal with alerts, adding MITRE tags, this can be done by the CSP or the system creating that alert. *But attaching a specific control to the alert, depends on many other factors that are not known to the system creating the alert.* For example: alert on a brute-force attempt on a cloud infrastructure -- the relevant tactics are simple. Controls over that type of alert can be multiple, and come from multiple areas -- those controls can be processes, procedures, or technological, they can be preventive or detective etc.

If we are not talking about alerts, but rather control deficiencies, such as lack of updates, firewall or antimalware not operating properly – *the recommended control is clear, but the relevant attacks or risks are not easily deducted* – a list of potential risks can be created, but this is dependent on many parameters (architecture, technology, accessibility etc.) so this is a complicated task that is being dealt with compliance systems.

**Scenarios**

As a SOC analyst, I need to be able to get contextual threat information and alerts from multiple CSPs.

1. what represents the greatest amount of risk – CSP informs part of that
2. asset that we try to protect – shared responsibility

Activity path - get alert -> validate whether false positive / true -> assess impact -> contextualize the alert -> resources and assets associated with alerts -> factors we are looking at -> high level data/functions these resources server -> categorize and prioritize -> identify the owner of the resource -> notify the owner -> determine appropriate action

**Alert examples**

**Example 1 -** (Medium severity) PREVIEW - Suspicious management session using an inactive account detected

**Provider**: Azure

**Tactic**: Persistence

**Resources**: Resource id, Subscription, User Principal Name (User account), Geolocation of the user associated with UPN, Client IP address / IP address associated with the UPN

**Suspicious action**: action points to resource on storage account
\[\"Microsoft.Storage/storageAccounts/listKeys/action\"\]

**Recommended actions**:

**More information link:**

**Example 2** - Activity insights security alert

**Provider:** IBM

**Tactic:**

**Resources:** provider resource id,

**Attacker / Actor:** user account

**Trigger / Suspicious action:**

**Event time**

**JSON CODE BLOCK**

```json
{

"outcome": "success",

"typeURI": "http://schemas.dmtf.org/cloud/audit/1.0/event",

"eventType": "activity",

"eventTime": "2021-01-28T04:50:52.51+0000",

"action": "security-advisor.findings.write",

"id": "3ac99528-bdd5-41ed-8729-68e9c37195f7",

"initiator": {

"id": "iam-ServiceId-5f4ca4ac-9a64-4bf0-ae03-4ad9a21afe42",

"name": "IBM Security Advisor",

"typeURI": "service/security/account/serviceid",

"host": {},

"credential": {

"type": "apikey"

}

},

"target": {

"id": "crn:v1::public:security-advisor:us-south:a/29104aa4ec94471284be7d33bf1b1391::occurrence:ata-1611809449329",

"name": "findingsapi",

"typeURI": "security-advisor/occurrence"

},

"observer": {

"name": "ActivityTracker",

"id": "activitytracker.ng.bluemix.net",

"typeURI": "service/security/edge/activity-tracker"

},

"reason": {

"reasonCode": 200,

"reasonType": "OK"

},

"requestData": {

"context": {

"region": "us-south",

"resource_name": "test-del",

"resource_type": "kms/secrets",

"service_crn": "crn:v1:bluemix:public:kms:us-south:a/f586c28d154d4c65a4a4a34cf75f55d0:bec392cc-97b0-45a1-9b94-e402de42e629::",

"service_name": "kms"

},

"finding": {

"certainty": "HIGH",

"next_steps": [

{

"title": "This finding was generated by an AT Event which was reported at 2021-01-28T04:32:55.46+0000 with action kms.secrets.delete"

}

]

},

"kind": "FINDING",

"id": "ata-1611809449329",

"note_name": "29104aa4ec94471284be7d33bf1b1391/providers/security-advisor/notes/ata-kms-key-deleted"

},

"responseData": {

"author": {

"account_id": "d642e8edf6b1e1b3e9de1f795f721a55",

"email": null,

"id": "iam-ServiceId-5f4ca4ac-9a64-4bf0-ae03-4ad9a21afe42",

"kind": "service-id"

},

"context": {

"account_id": "29104aa4ec94471284be7d33bf1b1391",

"region": "us-south",

"resource_name": "test-del",

"resource_type": "kms/secrets",

"service_crn": "crn:v1:bluemix:public:kms:us-south:a/f586c28d154d4c65a4a4a34cf75f55d0:bec392cc-97b0-45a1-9b94-e402de42e629::",

"service_name": "kms"

},

"create_time": "2021-01-28T04:50:52.487587Z",

"create_timestamp": 1611809452488,

"finding": {

"certainty": "HIGH",

"next_steps": [

{

"title": "This finding was generated by an AT Event which was reported at 2021-01-28T04:32:55.46+0000 with action kms.secrets.delete"

}

],

"severity": "HIGH"

},

"id": "ata-1611809449329",

"insertion_timestamp": 1611809452488,

"kind": "FINDING",

"long_description": "A change to KMS key was detected.",

"name": "29104aa4ec94471284be7d33bf1b1391/providers/security-advisor/occurrences/ata-1611809449329",

"note_name": "29104aa4ec94471284be7d33bf1b1391/providers/security-advisor/notes/ata-kms-key-deleted",

"provider_id": "security-advisor",

"provider_name": "29104aa4ec94471284be7d33bf1b1391/providers/security-advisor",

"reported_by": {

"id": "ata",

"title": "Activity insights"

},

"short_description": "KMS key change was detected.",

"update_time": "2021-01-28T04:50:52.487476Z",

"update_timestamp": 1611809452487,

"updateweekdate": "2021-W04-4",

"corelationId": "3407fe31-586e-4cf0-a123-825e43044936"

},

"severity": "normal",

"message": "Security Advisor: write findingsapi success",

"meta": {

"serviceProviderName": "security-advisor-dev",

"serviceProviderRegion": "ng",

"serviceProviderProjectId": "f781f06f-4ab3-45a3-916c-7806b3e81535",

"userAccountIds": [

"29104aa4ec94471284be7d33bf1b1391"

]

},

"logSourceCRN": "crn:v1:staging:public:security-advisor:us-south:a/29104aa4ec94471284be7d33bf1b1391:74128119-5107-5dbf-a2cf-5e9be228a243:security-advisor:",

"saveServiceCopy": true

}

[Action Item]{.ul}: Get some more examples from SOCs (Peter / Anatoliy / Janet / Preeti)

Example 3. AWS Example: Discovery:IAMUser/AnomalousBehavior

Tactics: Discovery TA0007

Resources: Access key

Attacker: Attacker UPN, Attacker geoloc, IP address

Suspicious Actions: type - AnomalousBehavior

[

{

"schemaVersion": "2.0",

"accountId": "123456789012",

"region": "us-east-1",

"partition": "aws",

"id": "0cbc8b3347ac538047fc56c2f4fe11a7",

"arn": "arn:aws:guardduty:us-east-1:123456789012:detector/3ab4a4be46389973d8f5f33bc86a29ea/finding/0cbc8b3347ac538047fc56c2f4fe11a7",

"type": "Discovery:IAMUser/AnomalousBehavior",

"resource": {

"resourceType": "AccessKey",

"accessKeyDetails": {

"accessKeyId": "ASIARZOOKWPXR24HLMYM",

"principalId": "AROAIHWLDLKQ2YARYAIIW:815945",

"userType": "AssumedRole",

"userName": "READONLYACCESS"

}

},

"service": {

"serviceName": "guardduty",

"detectorId": "3ab4a4be46389973d8f5f33bc86a29ea",

"action": {

"actionType": "AWSAPICALL",

"awsApiCallAction": {

"api": "DescribeClusters",

"serviceName": "redshift.amazonaws.com",

"callerType": "Remote IP",

"remoteIpDetails": {

"ipAddressV4": "74.80.52.132",

"organization": {

"asn": "25921",

"asnOrg": "LUS-FIBER-LCG",

"isp": "LUS Fiber",

"org": "LUS Fiber"

},

"country": {

"countryName": "United States"

},

"city": {

"cityName": "Lafayette"

},

"geoLocation": {

"lat": 30.209,

"lon": -92.0607

}

},

"affectedResources": {}

}

},

"resourceRole": "TARGET",

"additionalInfo": {

"userAgent": {

"fullUserAgent": "aws-internal/3 aws-sdk-java/1.11.965 Linux/4.9.230-0.1.ac.224.84.332.metal1.x8664 OpenJDK64-BitServerVM/25.282-b08 java/1.8.0282 vendor/OracleCorporation",

"userAgentCategory": "aws-internal/3"

},

"anomalies": {

"anomalousAPIs": "redshift.amazonaws.com:[DescribeClusters:success , DescribeClusterSubnetGroups:success , DescribeClusterSnapshots:success] , health.amazonaws.com:[DescribeEventAggregates:success]"

},

"profiledBehavior": {

"rareProfiledAPIsAccountProfiling": "",

"infrequentProfiledAPIsAccountProfiling": "",

"frequentProfiledAPIsAccountProfiling": "DescribeClusters , CreateLogStream , DescribeInstances , DescribeVolumes , DescribeNetworkInterfaces , DescribeTags , DescribeVpcs , ListAccountAliases , DescribeSecurityGroups , GetResources , DescribeSubnets , GetBucketAcl , DescribeLoadBalancers , UpdateInstanceInformation , DescribeAlarms , DescribeTable , ListFunctions20150331 , DescribeDBInstances , GetBucketLocation , ListBuckets",

"rareProfiledAPIsUserIdentityProfiling": "DescribeClusters , DescribeAccountAttributes , DescribeClusterSubnetGroups , ListSecrets , DescribeReservedNodes , DescribeAlarms , DescribeEvents , DescribeClusterSnapshots",

"infrequentProfiledAPIsUserIdentityProfiling": "GetResources",

"frequentProfiledAPIsUserIdentityProfiling": "DescribeEventAggregates",

"rareProfiledUserTypesAccountProfiling": "",

"infrequentProfiledUserTypesAccountProfiling": "",

"frequentProfiledUserTypesAccountProfiling": "ASSUMEDROLE , AWSSERVICE , ROLE",

"rareProfiledUserNamesAccountProfiling": "",

"infrequentProfiledUserNamesAccountProfiling": "",

"frequentProfiledUserNamesAccountProfiling": "READONLYACCESS , TAPDATASCIENTIST , AWSServiceRoleForAutoScaling , AWSServiceRoleForFMS , tenableio-connector , AWSServiceRoleForECS , AWSServiceRoleForComputeOptimizer , CUSTODIANEBSSNAPSHOT , eksctl-dmm-trial-cluster-ServiceRole-1L4F9KZTGGC6 , CUSTODIANSNS , TAPWorkerNode , AnalyticsAdminDMM , covid19-dynamostream-role-d4oaxatk , RESOURCETAGGING , AWSGlueDataBrewServiceRole-da-analytics , eksctl-dmm-triali-v17-cluster-ServiceRole-1V8OR5U5VMNSA , CUSTODIANIAMUSER , CUSTODIANEBS , AWSServiceRoleForTrustedAdvisor , CloudabilityRole",

"rareProfiledASNsAccountProfiling": "asnNumber: 396356 asnOrg: MAXIHOST asnNumber: 11776 asnOrg: ATLANTICBB-JOHNSTOWN asnNumber: 22773 asnOrg: ASN-CXA-ALL-CCI-22773-RDC asnNumber: 12271 asnOrg: TWC-12271-NYC asnNumber: 16591 asnOrg: GOOGLE-FIBER asnNumber: 10796 asnOrg: TWC-10796-MIDWEST",

"infrequentProfiledASNsAccountProfiling": "asnNumber: 11427 asnOrg: TWC-11427-TEXAS asnNumber: 6079 asnOrg: RCN-AS asnNumber: 26827 asnOrg: EPBTELECOM asnNumber: 20115 asnOrg: CHARTER-20115 asnNumber: 209 asnOrg: CENTURYLINK-US-LEGACY-QWEST",

"frequentProfiledASNsAccountProfiling": "asnNumber: 14618 asnOrg: AMAZON-AES asnNumber: 16509 asnOrg: AMAZON-02 asnNumber: 22616 asnOrg: ZSCALER-SJC1 asnNumber: 7922 asnOrg: COMCAST-7922 asnNumber: 7018 asnOrg: ATT-INTERNET4 asnNumber: 701 asnOrg: UUNET asnNumber: 46690 asnOrg: SNET-FCC asnNumber: 26292 asnOrg: ASN-SHREWS",

"rareProfiledASNsUserIdentityProfiling": "asnNumber: 22773 asnOrg: ASN-CXA-ALL-CCI-22773-RDC asnNumber: 701 asnOrg: UUNET asnNumber: 6079 asnOrg: RCN-AS asnNumber: 7922 asnOrg: COMCAST-7922 asnNumber: 22616 asnOrg: ZSCALER-SJC1 asnNumber: 12271 asnOrg: TWC-12271-NYC asnNumber: 16591 asnOrg: GOOGLE-FIBER",

"infrequentProfiledASNsUserIdentityProfiling": "",

"frequentProfiledASNsUserIdentityProfiling": "",

"rareProfiledUserAgentsAccountProfiling": "AWS CloudWatch Console , AWS ElasticMapReduce Console",

"infrequentProfiledUserAgentsAccountProfiling": "",

"frequentProfiledUserAgentsAccountProfiling": "AWS Service , OTHER , Botocore , aws-sdk-go , AWS Internal , aws-internal/3 , aws-sdk-java , AwsSignin , browser , aws-sdk-cloudwatchlogs , aws-sdk-nodejs , Amazon ECS Agent , aws-cli",

"rareProfiledUserAgentsUserIdentityProfiling": "aws-internal/3",

"infrequentProfiledUserAgentsUserIdentityProfiling": "AWS Service",

"frequentProfiledUserAgentsUserIdentityProfiling": "AwsSignin"

},

"unusualBehavior": {

"unusualAPIsAccountProfiling": "",

"unusualAPIsUserIdentityProfiling": "",

"unusualUserTypesAccountProfiling": "",

"unusualUserNamesAccountProfiling": "",

"unusualASNsAccountProfiling": "asnNumber: 25921 asnOrg: LUS-FIBER-LCG",

"unusualASNsUserIdentityProfiling": "asnNumber: 25921 asnOrg: LUS-FIBER-LCG",

"unusualUserAgentsAccountProfiling": "",

"unusualUserAgentsUserIdentityProfiling": "",

"isUnusualUserIdentity": "false"

}

},

"evidence": null,

"eventFirstSeen": "2021-04-28T02:41:25Z",

"eventLastSeen": "2021-04-28T02:41:25Z",

"archived": false,

"count": 1

},

"severity": 2,

"createdAt": "2021-04-28T02:54:50.712Z",

"updatedAt": "2021-04-28T02:54:50.712Z",

"title": "User AssumedRole : READONLYACCESS is anomalously invoking APIs commonly used in Discovery tactics.",

"description": "APIs commonly used in Discovery tactics were invoked by user AssumedRole : READONLYACCESS, under anomalous circumstances. Such activity is not typically seen from this user."

}

]

Example 4 - Based on SOC Feedback: AWS WAF Block - Provided by Peter Campbell, Cigna

#################### AWS WAFv2 Block ##############

{

"timestamp": 1621948354754,

"formatVersion": 1,

"webaclId": "arn:aws:wafv2:us-east-1:123456789012:regional/webacl/FMManagedWebACLV2Regional-WAFv2-Public1613136926787/b804e435-41ba-4ec3-8739-935234a522aa",

"terminatingRuleId": "PREFMManaged758ab7a6-aed4-4ae4-b688-eb7aaebf2c9f1",

"terminatingRuleType": "MANAGEDRULEGROUP",

"action": "BLOCK",

"terminatingRuleMatchDetails": [],

"httpSourceName": "APIGW",

"httpSourceId": "123456789012:ejwlrg1bkl:live",

"ruleGroupList": [{

"ruleGroupId": "arn:aws:wafv2:us-east-1:123456789012:regional/rulegroup/regional-wafv2-blocklist-public/95bd55a7-be5f-4246-bcaf-ae1456542b26",

"terminatingRule": null,

"nonTerminatingMatchingRules": [],

"excludedRules": null

},

{

"ruleGroupId": "AWS#AWSManagedRulesCommonRuleSet",

"terminatingRule": {

"ruleId": "GenericRFI_URIPATH",

"action": "BLOCK",

"ruleMatchDetails": null

},

"nonTerminatingMatchingRules": [],

"excludedRules": null

}

],

"rateBasedRuleList": [],

"nonTerminatingMatchingRules": [],

"requestHeadersInserted": null,

"responseCodeSent": null,

"httpRequest": {

"clientIp": "69.218.228.82",

"country": "US",

"headers": [{

"name": "X-Forwarded-For",

"value": "69.218.228.82"

},

{

"name": "X-Forwarded-Proto",

"value": "https"

},

{

"name": "X-Forwarded-Port",

"value": "443"

},

{

"name": "Host",

"value": "p-digital.digitaledge.XXXXXX.com"

},

{

"name": "X-Amzn-Trace-Id",

"value": "Root=1-60acf7c2-6164bd525be699a6776c15fe"

},

{

"name": "User-Agent",

"value": "Test Certificate Info"

},

{

"name": "Cache-Control",

"value": "no-cache"

}

],

"uri": "/https://p-digital.digitaledge.XXXXXX.com/",

"args": "",

"httpVersion": "HTTP/1.1",

"httpMethod": "HEAD",

"requestId": "f4umcGG7IAMFTYQ="

}

} {

"timestamp": 1621948487327,

"formatVersion": 1,

"webaclId": "arn:aws:wafv2:us-east-1:123456789012:regional/webacl/FMManagedWebACLV2Regional-WAFv2-Internal1613138918901/7ae7d539-1b0a-40ac-aeb6-c25ae8a3d4da",

"terminatingRuleId": "PREFMManagedbdf8c694-71d8-495b-a42d-deb8144d9f6d2",

"terminatingRuleType": "MANAGEDRULEGROUP",

"action": "BLOCK",

"terminatingRuleMatchDetails": [{

"conditionType": "SQL_INJECTION",

"location": "BODY",

"matchedData": [

"[BASE_64]H4sIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==",

"3",

"C",

"[BASE_64]I0sDM1MTQ1MLI0szJR00tUX5ZZkpqUUQxQYm5uaGhg=="

]

}],

"httpSourceName": "APIGW",

"httpSourceId": "123456789012:t4v04bwetc:live",

"ruleGroupList": [{

"ruleGroupId": "arn:aws:wafv2:us-east-1:123456789012:regional/rulegroup/regional-wafv2-internal/03de6be1-f6d4-4bae-9c62-158e0af9785c",

"terminatingRule": null,

"nonTerminatingMatchingRules": [],

"excludedRules": null

},

{

"ruleGroupId": "AWS#AWSManagedRulesCommonRuleSet",

"terminatingRule": null,

"nonTerminatingMatchingRules": [],

"excludedRules": null

},

{

"ruleGroupId": "AWS#AWSManagedRulesSQLiRuleSet",

"terminatingRule": {

"ruleId": "SQLi_BODY",

"action": "BLOCK",

"ruleMatchDetails": null

},

"nonTerminatingMatchingRules": [],

"excludedRules": null

}

],

"rateBasedRuleList": [],

"nonTerminatingMatchingRules": [],

"requestHeadersInserted": null,

"responseCodeSent": null,

"httpRequest": {

"clientIp": "10.189.113.193",

"country": "-",

"headers": [{

"name": "Host",

"value": "p-digital.internal.digitaledge.XXXXXX.com"

},

{

"name": "Content-Length",

"value": "133"

},

{

"name": "User-Agent",

"value": "python-requests/2.24.0"

},

{

"name": "Accept-Encoding",

"value": "gzip, deflate"

},

{

"name": "Accept",

"value": "*/*"

},

{

"name": "Authorization",

"value": ""

},

{

"name": "Content-Type",

"value": "application/json"

},

{

"name": "X-Request-Id",

"value": "dea13e40-71e1-49b7-9202-719a409ae676"

},

{

"name": "Content-Encoding",

"value": "gzip"

},

{

"name": "X-Forwarded-For",

"value": "10.189.113.193"

},

{

"name": "x-amzn-tls-version",

"value": "TLSv1.2"

},

{

"name": "x-amzn-cipher-suite",

"value": "ECDHE-RSA-AES128-GCM-SHA256"

},

{

"name": "x-amzn-vpce-id",

"value": "vpce-08ef3654f948a6153"

},

{

"name": "x-amzn-vpc-id",

"value": "vpc-01134f467b2c45ef0"

},

{

"name": "x-amzn-vpce-config",

"value": "1"

}

],

"uri": "/my/XXXXXX-reviews/v1/status",

"args": "",

"httpVersion": "HTTP/1.1",

"httpMethod": "POST",

"requestId": "f4u7KFVFoAMFXrQ="

}

Example 5 - Based on SOC feedback - Public Access to Object not Blocked - Provided by Peter Campbell, Cigna

namespace: onug.csnf.security.event.xxx

[

{

"schemaVersion": "2.0",

"accountId": "123456789012",

"region": "us-east-1",

"partition": "aws",

"id": "4cbcb3a0690db8fcac698e0bd3da65c5",

"arn": "arn:aws:guardduty:us-east-1:123456789012:detector/1BBD7aBAd3f8FfC6EF2Bb3CCE0EeBB2c/finding/4cbcb3a0690db8fcac698e0bd3da65c5",

"type": "Policy:S3/BucketBlockPublicAccessDisabled",

"resource": {

"resourceType": "AccessKey",

"accessKeyDetails": {

"accessKeyId": "ASIA46IH7B5JVYCXYB4N",

"principalId": "AROA46IH7B5J7BPCDZNTN:949237",

"userType": "AssumedRole",

"userName": "DEVELOPER"

},

"s3BucketDetails": [

{

"arn": "arn:aws:s3:::s3-medicare-reltio-json-outbound-temp",

"name": "s3-medicare-reltio-json-outbound-temp",

"defaultServerSideEncryption": null,

"createdAt": 1620934206,

"tags": [

{

"key": "AssetOwner",

"value": "dapda\@XXXXX.com"

},

{

"key": "DataOwner",

"value": "Matthew.Carroll\@XXXXX.com"

},

{

"key": "CiId",

"value": ""

},

{

"key": "DataSubjectArea",

"value": "Provider"

},

{

"key": "ResourceOwner",

"value": "Shankar.Subramani\@XXXXX.com"

},

{

"key": "ComplianceDataCategory",

"value": "phi:pii:hipaa"

},

{

"key": "DataCustodian",

"value": ""

},

{

"key": "Purpose",

"value": "Data Migration"

},

{

"key": "AppName",

"value": ""

},

{

"key": "AsaqId",

"value": ""

},

{

"key": "Project",

"value": "P"

},

{

"key": "CostCenter",

"value": "00790003"

},

{

"key": "DataClassification",

"value": "restricted"

},

{

"key": "BusinessDataCategory",

"value": "Provider"

}

],

"owner": {

"id": "5eabdf8f0a8f054c64e225b9e52b32290a8fd6163ed2a200416c2bccace4cc2e"

},

"publicAccess": {

"permissionConfiguration": {

"bucketLevelPermissions": {

"accessControlList": {

"allowsPublicReadAccess": false,

"allowsPublicWriteAccess": false

},

"bucketPolicy": {

"allowsPublicReadAccess": false,

"allowsPublicWriteAccess": false

},

"blockPublicAccess": {

"ignorePublicAcls": false,

"restrictPublicBuckets": false,

"blockPublicAcls": false,

"blockPublicPolicy": false

}

},

"accountLevelPermissions": {

"blockPublicAccess": {

"ignorePublicAcls": false,

"restrictPublicBuckets": false,

"blockPublicAcls": true,

"blockPublicPolicy": true

}

}

},

"effectivePermission": "NOT_PUBLIC"

},

"type": "Destination"

}

]

},

"service": {

"serviceName": "guardduty",

"detectorId": "1BBD7aBAd3f8FfC6EF2Bb3CCE0EeBB2c",

"action": {

"actionType": "AWSAPICALL",

"awsApiCallAction": {

"api": "PutBucketPublicAccessBlock",

"serviceName": "s3.amazonaws.com",

"callerType": "Remote IP",

"remoteIpDetails": {

"ipAddressV4": "165.225.220.243",

"organization": {

"asn": "22616",

"asnOrg": "ZSCALER-SJC1",

"isp": "Zscaler",

"org": "Zscaler"

},

"country": {

"countryName": "United States"

},

"city": {

"cityName": "Brooklyn"

},

"geoLocation": {

"lat": 40.7252,

"lon": -73.944

}

},

"affectedResources": {

"AWS::S3::Bucket": "s3-medicare-reltio-json-outbound-temp"

}

}

},

"resourceRole": "TARGET",

"additionalInfo": {},

"evidence": null,

"eventFirstSeen": "2021-05-13T19:31:48Z",

"eventLastSeen": "2021-05-13T19:31:48Z",

"archived": false,

"count": 1

},

"severity": 2,

"createdAt": "2021-05-13T19:42:51.931Z",

"updatedAt": "2021-05-13T19:42:51.931Z",

"title": "Amazon S3 Block Public Access was disabled for S3 bucket s3-medicare-reltio-json-outbound-temp.",

"description": "Amazon S3 Block Public Access was disabled for S3 bucket s3-medicare-reltio-json-outbound-temp by DEVELOPER calling PutBucketPublicAccessBlock. If this behavior is not expected, it may indicate a configuration mistake or that your credentials are compromised."

}

]

Example 6 - Based on SOC feedback - Object Exfil Unusual Object Reads - Provided by Peter Campbell, Cigna

[

{

"schemaVersion": "2.0",

"accountId": "123456789012",

"region": "us-east-1",

"partition": "aws",

"id": "f6bbe941e71df4233bb0ccf2b3c2114e",

"arn": "arn:aws:guardduty:us-east-1:123456789012:detector/30b4a4be02963c8c4af489ee69466725/finding/f6bbe941e71df4233bb0ccf2b3c2114e",

"type": "Exfiltration:S3/ObjectRead.Unusual",

"resource": {

"resourceType": "S3Bucket",

"accessKeyDetails": {

"accessKeyId": "ASIAXSC6MP3A525ZCU2A",

"principalId": "AROAISB2OBYT25RIR626O:391418",

"userType": "AssumedRole",

"userName": "DEVELOPER"

},

"s3BucketDetails": [

{

"owner": {

"id": "edb22168faa88922993a6841365882b349f44f4cd61a337959e72fc10b91af25"

},

"createdAt": 1582751796,

"publicAccess": {

"effectivePermission": "NOT_PUBLIC",

"permissionConfiguration": {

"accountLevelPermissions": {

"blockPublicAccess": {

"blockPublicPolicy": true,

"restrictPublicBuckets": false,

"blockPublicAcls": true,

"ignorePublicAcls": false

}

},

"bucketLevelPermissions": {

"accessControlList": {

"allowsPublicReadAccess": false,

"allowsPublicWriteAccess": false

},

"bucketPolicy": {

"allowsPublicReadAccess": false,

"allowsPublicWriteAccess": false

},

"blockPublicAccess": {

"blockPublicPolicy": true,

"restrictPublicBuckets": true,

"blockPublicAcls": true,

"ignorePublicAcls": true

}

}

}

},

"name": "imbigdata-dev-vendor-data-dnb-glue",

"defaultServerSideEncryption": {

"kmsMasterKeyArn": "arn:aws:kms:us-east-1:123456789012:key/2ac1cb5e-650c-4062-a0c7-80a69adba4a2",

"encryptionType": "aws:kms"

},

"arn": "arn:aws:s3:::imbigdata-dev-vendor-data-dnb-glue",

"type": "Destination",

"tags": [

{

"value": "silvio.galea\@XXXXX.com",

"key": "AssetOwner"

},

{

"value": "notAssigned",

"key": "CiId"

},

{

"value": "product",

"key": "DataSubjectArea"

},

{

"value": "DataAnalytics",

"key": "businessDataCategory"

},

{

"value": "s3",

"key": "module"

},

{

"value": "pii",

"key": "ComplianceDataCategory"

},

{

"value": "imbigdata-vendor-data-dnb ingestion pipeline",

"key": "Purpose"

},

{

"value": "Terraform",

"key": "heritage"

},

{

"value": "imbigdata-vendor-data-dnb",

"key": "AppName"

},

{

"value": "notAssigned",

"key": "AsaqId"

},

{

"value": "dev",

"key": "environment"

},

{

"value": "00007869",

"key": "CostCenter"

},

{

"value": "1.0.8",

"key": "moduleVersion"

},

{

"value": "7 years",

"key": "DataRetentionCode"

},

{

"value": "restricted",

"key": "DataClassification"

},

{

"value": "us-east-1:us-east-2:us-west-1:us-west-2:",

"key": "RegionalRestriction"

}

]

}

]

},

"service": {

"serviceName": "guardduty",

"detectorId": "30b4a4be02963c8c4af489ee69466725",

"action": {

"actionType": "AWSAPICALL",

"awsApiCallAction": {

"api": "GetObject",

"serviceName": "s3.amazonaws.com",

"callerType": "Remote IP",

"remoteIpDetails": {

"ipAddressV4": "104.129.202.52",

"organization": {

"asn": "22616",

"asnOrg": "ZSCALER-SJC1",

"isp": "Zscaler",

"org": "Zscaler"

},

"country": {

"countryName": "United States"

},

"city": {

"cityName": "Dublin"

},

"geoLocation": {

"lat": 37.7201,

"lon": -121.919

}

},

"affectedResources": {}

}

},

"resourceRole": "TARGET",

"additionalInfo": {},

"evidence": null,

"eventFirstSeen": "2021-02-24T05:22:54Z",

"eventLastSeen": "2021-02-24T22:36:28Z",

"archived": false,

"count": 9

},

"severity": 5,

"createdAt": "2021-02-24T05:29:49.379Z",

"updatedAt": "2021-02-24T22:42:10.987Z",

"title": "Unusual reads of objects in an S3 bucket.",

"description": "A principal read objects from an S3 bucket in an unusual way."

}

]

Example 7 - Azure ApplicationGatewayFirewall, IP Block, Peter Campbell, Cigna

{

"timeStamp": "2021-06-01T10:55:20+00:00",

"resourceId": "/SUBSCRIPTIONS/30EB8E23-D885-4F27-AEB4-123456789/RESOURCEGROUPS/INTL-XXXXX-PROD-RG/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/INTL-XXXXX-PROD_AGW",

"operationName": "ApplicationGatewayFirewall",

"category": "ApplicationGatewayFirewallLog",

"properties": {

"instanceId": "appgw_2",

"clientIp": "34.77.162.6",

"requestUri": "/",

"ruleSetType": "Custom",

"ruleId": "allowCountryList",

"message": "Found condition 0 in RemoteAddr, with value 34.77.162.6. Found condition 1 in RemoteAddr, with value 34.77.162.6. Found condition 2 in RemoteAddr, with value 34.77.162.6. Found condition 3 in RemoteAddr, with value 34.77.162.6.",

"action": "Blocked",

"hostname": "roiwt-api.XXXXX.com",

"transactionId": "47b3399b39e79848fa0b55cd22f10f97",

"policyId": "9#_subscriptions30eb8e23-d885-4f27-aeb4-123456789resourceGroupsintl-XXXXX-prod-rgprovidersMicrosoft.NetworkApplicationGatewayWebApplicationFirewallPolicies_coe-wafpolicy",

"policyScope": "Listener",

"policyScopeName": "prod-roiwt-httplstn",

"engine": "Azwaf"

}

}

Example 8 - GCP Events from Kathryn

[https://cloud.google.com/security-command-center/docs/concepts-vulnerabilities-findings]{.ul}

Example 9 - Peter to add some GCP examples from SOC

Mark - to get more examples from the consumers added to this - preferred by - 6/17 workshop

Anton - additional examples of various security events

Canonical definitions

[Define Namespace]{.ul}

Requirements:

P0 - unique and stable

P0 - needs to be expandable.

P1 - Minimal oversight and registration and lightweight

Provisions in place for private / customizations outside the public

e.g. onug.csnf.myproduct.security.event.ipaddress
onug.csnf.identity.event.xxx, onug.csnf.compliance.event.xxx

onug.csnf.identity.policy.xxx, onug.csnf.security.state.xxx, onug.csnf.identity.asset.xxx, etc.
Tracking of products customizing the namespace and publishing it for

others' usage - to avoid collisions and visibility into company using it
Protections for the namespace

idea 1 - onug.csnf.security.event.ipaddress, onug.csnf.security.monitoring.xxx,

[Action item -]{.ul} get community feedback on namespace

[Definitions]{.ul}

Provider: provider name

Event name: name of event

Event identifier: id of the event

Event start time: time when event occurred (time format set?)

Event end time (optional): time when event ended (time format

set?) - could be ambiguous as event may restart
Event type: enum of type - for e.g. Threat, WAF, etc. for

security namespace (scoping currently to security and later scale out to identity, compliance, etc. scenarios).
References (optional): attack tactics from the alert / event [-

can be used for custom enrichment - Needs further drill down]{.ul}
a. Source (MITRE/NIST/CVE)

b. Identifier

c. Name

Targeted Resources (optional):

a. Provider Resource id

b. Account Id

c. Additional labels (e.g. tags/keys, dictionary value, > compartments)

d. User Principal Name (User account)

e. Account type (e.g. service, authenticated user, etc.)

f. Geolocation - country, state, city, zip

g. Client IP address

h. Access Key

i. Agent type

Actor (optional): is the attacker

a. Provider Resource id

b. Account Id

c. Additional labels (e.g. tags/keys, dictionary value, > compartments)

d. User Principal Name (User account)

e. Account type (e.g. service, authenticated user, etc.)

f. Authentication Type

g. Geolocation

h. Client IP address

i. Access Key

j. Agent type

Trigger

a. TriggerName (optional) - could this be an enum with 'other' > as catch-all?: suspicious action that triggered the > event - do we want to tie this to owasp top10 security? Should > the trigger be generalized to a high level category with > description for details. For e.g. policy changes in a less > secure way (e.g. PII, HIPPA, improper location), violations to > security (actions that violate security like SQL > injection,...)

b. TriggerDescription (optional): do we need this (on the > fence - Peter to look at some more examples to see whether we > need this or just trigger is enough).

c. TriggerSource (e.g. product, other engine, etc.)

Recommended actions (optional): what are the next steps

More information link: links to original event on provider side

Stages

Ingest / Pre-analytic

Post analytic / Automations

Future: Saas models,

[Fall Demo]{.ul}

Vulnerable container →

Scenario #1 - Vulnerable container in image repo

Scenario #2 - Container - Vulerability scanner goes in

Deployed and multi cloud

SOC analyst → ONUGUser - Security Analyst

[Requirements]{.ul}

QA environment - needed

Roles / Personas

Security analyst

Developer - pushes piece of code

Vulnerability Management Engineer

Demo #1 - Simpler - Showcase schema from multiple CSPs and show in common schema that's standardized and enriched

decorate the alert with different info, array of decorations

Demo #2: Standardization scenario: Simple flow - generate event that generates log in multi-cloud - Generating events in a natural mode

CVE-ID kind of info and show the experience of an analyst - how it > saves time for them. SDK to leverage time savings
CVEs a bunch of those are bundled in with each resource - so how to get to actionable data from this? SOC analyst receives an alert, CVE, MITRE info is standardized and provided in canonical format across multi-cloud logs.

These can be represented as a graph - how to visualize the graph, etc. would need to be thought about. Group by resources / CVEs and based on the view have appropriate actions based on severities and policies.

Security analysts can still cross-correlate and show power of

standardization
Resource identification - enrichment from CMDb
get contextual info IaaS type services on resources etc. info, disallowed ports, identify configs across providers easily.

Demo #3 : Standardization + Enrichment scenario: SOC analyst detects vulnerability → Get the scenario complete (Anatoliy / Preeti)

Persona: SOC Analyst looks at

Multi clouds are involved

Show flow of alert from one cloud and another cloud - receives

notifications from multi clouds
SOC analyst investigates and uses metadata / SDK to get to direct

outcomes to the root cause → can even look at automated actions?
MITRE attack is key piece to
CSP scenarios for demo

GCP Security command center - API detects these security issues.

Have a design that doesn't have a log like behavior. Subscription needed for this.
May need a collector product (PAN Prisma, etc.) to showcase to pull the logs and show the use case →

Deploy malicious code - [https://davisanc.github.io/lab.aks.io/]{.ul} or we develop our own test suite

Next workshop agenda

Show products from GCP, IBM, Azure to showcase the logs emit as well > as consumption product as well. Anton/Kathryn/Preeti/Anatoliy)
Demo #2 - Developer pushes bad code and that's detected by scanner and shows up as vulnerability

Appendix

{

"feedback": null,

"recommendedActions": [],

"networkConnections": [],

"detectionIds": [],

"id": "91azzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz",

"category": "UnfamiliarLocation",

"confidence": null,

"fileStates": [],

"severity": "medium",

"title": "Unfamiliar sign-in properties",

"sourceMaterials": [],

"comments": [],

"assignedTo": "",

"eventDateTime": "2019-05-01T23:57:49.4122069Z",

"activityGroupName": null,

"status": "newAlert",

"description": "Sign-in with properties not seen recently for the given user",

"tags": [],

"lastModifiedDateTime": "2019-05-02T00:04:42.7836811Z",

"historyStates": [],

"vendorInformation": {

"providerVersion": "3.0",

"vendor": "Microsoft",

"subProvider": null,

"provider": "IPC"

},

"userStates": [

{

"userPrincipalName": "someone@.somewhere.com",

"emailRole": "unknown",

"isVpn": null,

"userAccountType": null,

"domainName": null,

"onPremisesSecurityIdentifier": null,

"aadUserId": null,

"accountName": "AccountName",

"logonIp": "123.456.789.1",

"logonDateTime": null,

"logonType": null,

"logonLocation": "Seattle, Washington, US",

"riskScore": "0",

"logonId": null

}

],

"malwareStates": [],

"processes": [],

"azureTenantId": "sanitized - GUID",

"registryKeyStates": [],

"createdDateTime": "2019-05-01T23:57:49.4122069Z",

"triggers": [],

"hostStates": [],

"cloudAppStates": [],

"closedDateTime": null,

"riskScore": "",

"azureSubscriptionId": null,

"vulnerabilityStates": []

}
```


