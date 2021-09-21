---
title: "Use Cases"
linkTitle: "Use Cases"
weight: 102
description: >-
     CSNF: Use Cases
---

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
