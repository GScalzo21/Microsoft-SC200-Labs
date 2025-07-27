# 🛡️ Microsoft Defender for Endpoint Deployment Lab  
**SC-200: Microsoft Security Operations Analyst – Deploy Microsoft Defender for Endpoint**

## Overview

This hands-on exercise focused on deploying Microsoft Defender for Endpoint (MDE) to strengthen security visibility, improve response capabilities, and implement structured device grouping. The goal was to onboard devices, configure access roles, and prepare the Defender XDR environment for operational readiness in a realistic enterprise setting.
<img width="1352" height="575" alt="Screenshot 2025-07-26 at 8 27 08 PM" src="https://github.com/user-attachments/assets/e706a8e3-1c62-4d19-8a2b-7b1f7770e35c" />


## Scenario

As a Security Operations Analyst, I was tasked with helping an organization onboard devices into Microsoft Defender for Endpoint. The objective was to provide visibility for the SecOps team while configuring access controls through custom roles and device groups. This is a core task in any modern SOC, and the lab mirrored real-world processes used in production environments.

---

## Task 1: Initializing Microsoft Defender for Endpoint

I began by signing into the Microsoft Defender XDR portal at [https://security.microsoft.com](https://security.microsoft.com) using the tenant admin credentials provided. Once inside, I navigated to:

- **Settings > Device discovery**
- Selected **Standard discovery** as the preferred method

This option allows Defender to scan the network and detect unmanaged or unmonitored endpoints, which is essential for full visibility and reducing blind spots in the environment.

---

## Task 2: Onboarding a Device Using Local Script

To simulate onboarding endpoints into MDE:

1. Navigated to **Settings > Endpoints > Onboarding**
2. Selected **Local script (up to 10 devices)** as the deployment method
3. Downloaded the `WindowsDefenderATPOnboardingPackage.zip`
4. Extracted the contents and right-clicked the onboarding script to unblock it under file properties
5. Executed the script (`WindowsDefenderATPLocalOnboardingScript.cmd`) as Administrator

After running the script, I received confirmation that the machine had been successfully onboarded to Microsoft Defender for Endpoint. This confirmed that telemetry from the endpoint would now begin flowing into Defender XDR.

---

## Task 3: Configuring a Custom Role

To implement granular access control:

1. Navigated to **Settings > Microsoft Defender XDR > Permissions & Roles**
2. Created a custom role named **Tier 1 Support**
3. Under *Security Operations*, granted **all read and manage permissions**
4. Assigned the `sg-IT` group to this role

This ensures only authorized personnel within the security team have the appropriate level of access to device data and incident workflows, aligning with the principle of least privilege.

---

## Task 4: Creating Device Groups

Device groups enable segmentation of endpoints by criteria such as operating system or business unit. I created a group to reflect a real-world environment:

1. Went to **Settings > Endpoints > Device groups**
2. Added a new group named **Regular**
3. Set **Remediation level** to *Full remediation*
4. Filtered for devices running **Windows 11**
5. Assigned Azure AD group `sg-IT` as the authorized user group

After submitting the configuration, I verified that both the new group and the default ungrouped devices group were present.

---

## Key Concepts Practiced

- Endpoint onboarding via secure script deployment
- Role-based access control (RBAC) implementation
- Device group creation and filtering by OS
- Standard discovery configuration for visibility
- Use of the Defender XDR portal for operational setup

---

## Relevance to SC-200

This lab reinforced the following exam objectives:

- Managing the Microsoft Defender for Endpoint environment
- Configuring onboarding and permissions
- Implementing RBAC and device grouping
- Enhancing organizational visibility through telemetry ingestion

---

## Author  
**Giuseppe Scalzo**  
GitHub: [@GScalzo21](https://github.com/GScalzo21)  
