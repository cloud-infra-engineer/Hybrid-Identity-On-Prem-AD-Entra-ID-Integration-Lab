# Hybrid Identity — On-Prem AD & Entra ID Integration Lab

## Business Problem

Organisations modernising to the cloud rarely start with a clean slate — most still depend on legacy on-premises infrastructure (file servers, older line-of-business applications, payroll systems) that's tied to Active Directory, while also needing modern cloud applications secured through Entra ID. Maintaining two completely separate identity systems means employees hold two sets of credentials, IT manages two separate onboarding/offboarding processes, and access can drift out of sync between the two environments — creating both a poor user experience and a real security and audit risk.

## Solution Overview

Built a hybrid identity environment by deploying Active Directory Domain Services (AD DS) inside an Azure VM, then synchronising it to Microsoft Entra ID using Entra Connect/Cloud Sync. This creates a single, authoritative identity for each user that works consistently across both legacy on-premises systems and modern cloud applications — rather than maintaining separate, disconnected identities in each environment.

## Scope Note

This project focuses on establishing hybrid identity synchronisation between on-premises AD and Entra ID, and layering modern access controls (Conditional Access, MFA, PIM) on top of the synchronised identities.

[Placeholder — update as you decide what's in/out of scope, e.g. access reviews, full governance certification, covered separately elsewhere.]

## Architecture

![Architecture Diagram](hybrid-ad-entra-architecture.png)

## What I Built

**AD DS Setup:** [Placeholder — describe what you configured: domain name, VM setup, promoting the server to a domain controller.]

**Entra Connect / Cloud Sync Configuration:** [Placeholder — describe how you connected on-prem AD to Entra ID.]

**Sync Verification:** Confirmed that user accounts created directly in on-premises AD DS synchronised successfully to Entra ID — verified by checking Entra ID's Users list and confirming the same accounts appear there automatically, without manual creation in the cloud.

*(Conditional Access, MFA, and PIM sections to be added as the lab progresses.)*

## Troubleshooting & Problems I Hit

[Placeholder — log issues as you hit them, same as the IGA lab.]

## Business Outcome

[Placeholder — to be completed once Conditional Access/MFA/PIM sections are built out.]
