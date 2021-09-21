---
title: "Consumer Experience"
linkTitle: "Consumer Experience"
weight: 100
description: >-
     Desired Consumer Experience
---

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
