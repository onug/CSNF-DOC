---
title: "Alerts, Controls, and Risks"
linkTitle: "Alerts, Controls, and Risks"
weight: 105
description: >-
     Page description for Alerts, Controls, and Risks.
---

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
