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

## Password Hash Synchronization (PHS)

**Business case:** Without hybrid identity, an employee would need separate credentials for on-premises resources and cloud/SaaS applications — one username and password to log into their on-prem-connected systems, and a completely different one for cloud apps like Microsoft 365 or other SaaS tools. This creates a worse user experience, more passwords to manage and forget, and more accounts for IT to secure. Password Hash Synchronization solves this by allowing an employee to use the same on-premises username and password to authenticate into cloud resources — a single identity, working consistently across both environments.

**What was built:** Configured Entra Connect to synchronise on-premises Active Directory with Entra ID, with Password Hash Synchronization enabled. This requires Entra Connect specifically — PHS is a feature of the sync process itself, not something configured independently.

**A caveat worth naming:** this only works cleanly when the on-premises domain matches a verified domain in the Entra ID tenant. In this lab, the on-prem domain (`contoso.com`) doesn't match the tenant's original sign-up domain, so Entra Connect falls back to mapping synced users to the tenant's default `.onmicrosoft.com` domain instead. In a real enterprise deployment, the on-prem domain would typically be a verified, owned domain matching the cloud tenant, allowing a fully consistent sign-in experience across both environments.

**Verification:** Confirmed this worked correctly through three layers of proof, not just one:

![L1 signed in as synced user](L1%20sign%20in.png)
![VM visible but not running — Reader access confirmed](vm%20not%20running.png)
![Failed authorisation — L1 blocked from starting the VM](Failed%20authorisation.png)

1. **Sync** — users created directly in on-premises AD appeared automatically in both Entra ID and Azure, with no manual creation required in the cloud.
2. **Authentication** — logged in as one of these synced users via the `.onmicrosoft.com` UPN (per the domain caveat above) using the exact password set on-premises, and the login was accepted — confirming the password hash had synced correctly and could authenticate against Entra ID.
3. **Authorization** — assigned the user a Reader role in both Entra ID and Azure, then tested the boundary of that permission two ways: positively, by confirming the user could view users and subscriptions as expected; and negatively, by attempting to create a virtual machine, which was correctly blocked as an action Reader-level access doesn't permit. This negative test is the stronger proof — it confirms the permission boundary was genuinely enforced, not just that some access existed.

Together, these three layers confirm Password Hash Synchronization works end-to-end: a single on-premises identity, successfully synced, authenticated, and correctly authorized into cloud resources.

## Troubleshooting & Problems I Hit

**Issue: Reader roles assigned but user had no access**

Assigned a test user Reader roles in both Entra ID and at the Azure subscription level, but on logging in as that user, they couldn't see any existing resources — Azure even prompted them to start a new free trial, as if the account had no relationship to the existing subscription at all.

Investigated by checking the role assignments directly and found both roles had been created as **Eligible** rather than **Active** — a PIM (Privileged Identity Management) setting. An Eligible assignment means the user is permitted to hold the role, but it isn't actually in effect until they manually activate it themselves; nothing had been activated, so the user genuinely had no access at all, despite the assignment technically existing.

Resolved by removing both eligible assignments and reassigning them as Active, which took effect immediately and was confirmed by successfully logging in as the user and seeing the existing VM. This was a useful hands-on demonstration of a real PIM concept — access isn't standing by default, it must be deliberately granted or activated — rather than just a lab misconfiguration to fix and move past.

## Business Outcome

[Placeholder — to be completed once Conditional Access/MFA/PIM sections are built out.]
