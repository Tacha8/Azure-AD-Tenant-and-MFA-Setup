# Azure AD Tenant Deployment with MFA & Identity Administration

In this lab, I created a dedicated Azure AD (Entra ID) tenant and secured it by enabling Multi-Factor Authentication (MFA) for the global admin account. I also installed and connected PowerShell 7, the Microsoft Graph module, and the Azure CLI to manage the tenant through command-line tools instead of just the GUI.

This lab simulates how IAM engineers set up a secure and manageable identity environment from scratch.

## Objectives

By the end of this lab, I completed the following:

-Created a separate Azure AD tenant
-Set up and secured a Global Administrator account with MFA
-Installed and authenticated via:
-PowerShell 7
-Microsoft Graph PowerShell Module
-Azure CLI
-Verified the tenant setup using CLI commands

---

<img width="1400" height="808" alt="image" src="https://github.com/user-attachments/assets/b8bfc939-bbfb-4c12-9c53-c97d23b31919" />


# Tools Used

-Azure Portal
-PowerShell 7
-Microsoft Graph PowerShell
-Azure CLI
-Microsoft Authenticator App (for MFA)

---


# Table of Contents

- [Vulnerability Management Policy Draft Creation](#vulnerability-management-policy-draft-creation)
- [Mock Meeting: Policy Buy-In (Stakeholders)](#step-2-mock-meeting-policy-buy-in-stakeholders)
- [Policy Finalization and Senior Leadership Sign-Off](#step-3-policy-finalization-and-senior-leadership-sign-off)
- [Mock Meeting: Initial Scan Permission (Server Team)](#step-4-mock-meeting-initial-scan-permission-server-team)
- [Initial Scan of Server Team Assets](#step-5-initial-scan-of-server-team-assets)
- [Vulnerability Assessment and Prioritization](#step-6-vulnerability-assessment-and-prioritization)
- [Distributing Remediations to Remediation Teams](#step-7-distributing-remediations-to-remediation-teams)
- [Mock Meeting: Post-Initial Discovery Scan (Server Team)](#step-8-mock-meeting-post-initial-discovery-scan-server-team)
- [Mock CAB Meeting: Implementing Remediations](#step-9-mock-cab-meeting-implementing-remediations)
- [Remediation Round 1: Outdated Wireshark Removal](#remediation-round-1-outdated-wireshark-removal)
- [Remediation Round 2: Insecure Protocols & Ciphers](#remediation-round-2-insecure-protocols--ciphers)
- [Remediation Round 3: Guest Account Group Membership](#remediation-round-3-guest-account-group-membership)
- [Remediation Round 4: Windows OS Updates](#remediation-round-4-windows-os-updates)
- [First Cycle Remediation Effort Summary](#first-cycle-remediation-effort-summary)

---

### Created a brand-new Azure AD tenant

To start the lab, I signed in to the Azure Portal and created a brand-new Entra ID (Azure AD) tenant instead of using my personal account. I went to Azure Active Directory → Manage tenants → Create, selected Azure AD as the tenant type, and entered the organization name, domain (onmicrosoft.com), and region. After deployment finished, I switched into the new tenant using the “Switch tenant” option. This gave me an isolated identity environment where I could configure users, MFA, and admin settings without mixing anything with a personal account.  

<img width="963" height="326" alt="image" src="https://github.com/user-attachments/assets/0345d0c7-341b-4a6b-b91a-5fa64bef3caf" />


---

### Added a Global Administrator account

To secure and manage the new tenant properly, I created a cloud-only Global Administrator account instead of using my personal Microsoft identity. From the Entra ID portal, I went to Users → New user → Create new user, entered the name and username in the tenant’s domain, and let Azure generate a temporary password. After creating the account, I confirmed it showed up under the tenant’s user list and made sure it was recognized as a directory-native identity instead of an external account. This account became the primary admin for all future IAM configuration and security tasks.

<img width="1357" height="607" alt="image" src="https://github.com/user-attachments/assets/0dbe7da0-0fde-4ad0-a3e3-23cf6cccdeee" />

---

### Step 3) Enabled MFA for the admins

After creating the Global Administrator account, I immediately enabled MFA to protect high-privilege access. In the Entra ID portal, I went to Users → Per-user MFA settings, selected the admin account, and enforced MFA. When I signed in with the new credentials, Azure prompted me to register the Microsoft Authenticator app and complete verification. Enabling MFA at this stage ensures that even if someone gains the admin password, they still can’t access the tenant without the second authentication factor.

<img width="1423" height="357" alt="image" src="https://github.com/user-attachments/assets/c349a75a-b236-43bd-86f6-ec3f987a7d28" />


---

### Step 4) Mock Meeting: Initial Scan Permission (Server Team)

The team collaborates with the server team to initiate scheduled credential scans. A compromise is reached to scan a single server first, monitoring resource impact, and using just-in-time Active Directory credentials for secure, controlled access.  

**Ti:** Good morning, Mike. Ready to conduct some scans?  

**Mike:** Yep. Now that our vulnerability management policy is in place, I’d like to start scheduling credentialed scans of your environment.  

**Ti:** Sounds good. What’s involved, and how can we help?  

**Mike:** We’re planning weekly scans of the server infrastructure. We estimate it will take 4–6 hours to scan all 2,200 assets. For this, we’ll need administrative credentials so the scan engine can remotely log in and assess systems more accurately.  

**Ti:** Hold on—what exactly does scanning entail? I’m concerned about resource utilization. And you’re asking for admin credentials to all 2,200 machines—that doesn’t sound safe.  

**Mike:** Those are valid concerns. The scan engine sends controlled traffic to the servers to check for vulnerabilities. It looks at things like registry settings, outdated software, and insecure protocols or cipher suites. Credentials are required so the scan can perform deeper checks, but it won’t take systems offline.  

**Ti:** Okay, as long as it won’t bring down the servers, that should be fine. How about we start small—scan a single server first and monitor resource utilization?  

**Mike:** That’s a good idea. Also, for credentials, could you set up a dedicated account in Active Directory? You could keep it disabled until scanning time, then enable it during the scan, and disable or deprovision it afterward. That way it’s “just-in-time” access.  

**Ti:** That works. I’ll ask Susan to start automation for provisioning that account.  

**Mike:** Perfect. Talk soon.  

**Ti:** Sounds good. I’ll get back to you once the credentials are set up.  

**Mike:** Great. See you later.  

**Ti:** See you. 

---

### Step 5) Initial Scan of Server Team Assets

In this phase, an insecure Windows Server is provisioned to simulate the server team's environment. After creating vulnerabilities, an authenticated scan is performed, and the results are exported for future remediation steps.  

<img width="918" height="683" alt="image" src="https://github.com/user-attachments/assets/4a248da4-4ebb-48cd-bc61-8e4aa88e4da9" />


[Scan 1 - Initial Scan](https://drive.google.com/file/d/17YyCuBLup13agA9TX2AulcKitP3udzFY/view?usp=sharing)




---

### Step 6) Vulnerability Assessment and Prioritization

We assessed vulnerabilities and established a remediation prioritization strategy based on ease of remediation and impact. The following priorities were set:

1. Third Party Software Removal (Wireshark)
2. Windows OS Secure Configuration (Protocols & Ciphers)
3. Windows OS Secure Configuration (Guest Account Group Membership)
4. Windows OS Updates

---

### Step 7) Distributing Remediations to Remediation Teams

The server team received remediation scripts and scan reports to address key vulnerabilities. This streamlined their efforts and prepared them for a follow-up review.  

<img width="635" alt="image" src="https://github.com/user-attachments/assets/bbf9478f-e1d1-4898-846e-b510ec8c6f72">

[Remediation Email](https://github.com/joshmadakor1/lognpacific-public/blob/main/misc/remediation-email.md)

---

### Step 8) Mock Meeting: Post-Initial Discovery Scan (Server Team)

The server team reviewed vulnerability scan results, identifying outdated software, insecure accounts, and deprecated protocols. The remediation packages were prepared for submission to the Change Control Board (CAB). 

**Ti:** Morning, Mike. How are you doing?  

**Mike:** Not bad for a Monday. How about you?  

**Ti:** Still alive, so I can’t complain. Before we dive into vulnerabilities, how did the scan go on your end? Any outages or resource issues?  

**Mike:** The scan went well. We were monitoring the servers, and aside from noticing a lot of open connections, you wouldn’t have known a scan was running.  

**Ti:** That’s good news—about what I expected. We’ll keep monitoring, but I don’t anticipate any resource utilization problems. Mind if I go over the findings?  

**Mike:** Absolutely.  

**Ti:** Great. I’ll share my screen. Most of the vulnerabilities stem from Wireshark being installed—it’s just very outdated. Another issue I found is that the local Guest account on the servers is part of the Local Administrators group, which is concerning. Some vulnerabilities, like those tied to Microsoft Edge Chromium, may resolve automatically through Windows Updates. The self-signed certificate finding isn’t a real concern since it’s just the system’s own certificate.  

The bigger issues are medium-strength cipher suites and deprecated protocols like TLS 1.0 and 1.1. Those should definitely be remediated. So in summary: remove outdated Wireshark installs, address insecure protocols and cipher suites, and fix the Guest account.  

**Mike:** Interesting. The good news is most of our servers probably share the same vulnerabilities, so remediation should be uniform.  

**Ti:** Exactly—it’s easier when the issues are consistent. Do you foresee any problems fixing things like the cipher suites or protocols?  

**Mike:** I doubt it. We’ll run everything through the next Change Control Board. Removing Wireshark and fixing the Guest account shouldn’t be an issue—those shouldn’t be on the servers anyway. I’ll check with our CIS admins on that.  

**Ti:** Sounds good. I’ll build out some remediation packages to streamline the process for your team.  

**Mike:** Great, thanks. One question—do you already have something in place for patching the Windows Update-related vulnerabilities?  

**Ti:** Yes, patch management is in place. Windows Updates should take care of those automatically by next week.  

**Mike:** Excellent.  

**Ti:** Perfect. I’ll start researching the best ways to remediate the other findings and get back to you before the next Change Control Board meeting.  

**Mike:** Sounds good. Talk to you soon.  

**Ti:** Talk to you soon.

---

### Step 9) Mock CAB Meeting: Implementing Remediations

The Change Control Board (CAB) reviewed and approved the plan to remove insecure protocols and cipher suites. The plan included a rollback script and a tiered deployment approach.  

**Facilitator:** Next up are a couple of vulnerability remediations for the server team:  
1. Removal of insecure protocols  
2. Removal of insecure cipher suites  

It looks like **Ti** from the Risk Department has been working with **Mike** from Infrastructure on this. Mike, would you like to walk us through the technical aspects?  

**Mike:** Normally I would, but in this case Ti actually built the solution. We’re still getting used to the process, so I’ll let him explain.  

**Ti:** Sure. Insecure cipher suites and protocols mean the system is still capable of negotiating outdated or deprecated encryption methods. If a server only supports those, the computer might use them, which is a security risk.  

These settings are controlled in the Windows Registry. The fix is straightforward—we wrote a PowerShell script that disables the insecure protocols and ciphers while enabling only secure, modern standards.  

**Facilitator:** Sounds good, but what if something goes wrong? Do we have a rollback plan?  

**Ti:** Absolutely. We’re doing a tiered deployment: first a pilot group of machines, then pre-production, and finally production. In addition, we’ve built automated rollback scripts for each remediation. If any issues arise, the script will restore the original protocols and ciphers.  

**Facilitator:** That’s reassuring. Since these are just registry updates, I’m not too concerned.  

**Ti:** Exactly—it’s simple but effective.  

**Facilitator:** Great. Any more questions? … None? Then that wraps up this week’s CAP meeting. See you all next week.  

**All:** See you.

---
### Step 10 ) Remediation Effort

#### Remediation Round 1: Outdated Wireshark Removal

The server team used a PowerShell script to remove outdated Wireshark. A follow-up scan confirmed successful remediation.  
[Wireshark Removal Script](https://github.com/joshmadakor1/lognpacific-public/blob/main/automation/remediation-wireshark-uninstall.ps1)  

<img width="920" height="749" alt="image" src="https://github.com/user-attachments/assets/d121b5c0-a47c-4549-852c-b00c29a49102" />


[Scan 2 - Third Party Software Removal](https://drive.google.com/file/d/12zpJslq4BgXF2WSkikH-P2Pf10gWq223/view?usp=sharing)


#### Remediation Round 2: Insecure Protocols & Ciphers

The server team used PowerShell scripts to remediate insecure protocols and cipher suites. A follow-up scan verified successful remediation, and the results were saved for reference.  
[PowerShell: Insecure Protocols Remediation](https://github.com/joshmadakor1/lognpacific-public/blob/main/automation/toggle-protocols.ps1)
[PowerShell: Insecure Ciphers Remediation](https://github.com/joshmadakor1/lognpacific-public/blob/main/automation/toggle-cipher-suites.ps1)

<img width="917" height="683" alt="image" src="https://github.com/user-attachments/assets/3436055b-dd3d-4d54-9403-d1785ec4e352" />


[Scan 3 - Ciphersuites and Protocols](https://drive.google.com/file/d/1I2ATDl4GVkr95IIeD8WbEjZqQZ0l9sUk/view?usp=sharing)


#### Remediation Round 3: Guest Account Group Membership

The server team removed the guest account from the administrator group. A new scan confirmed remediation, and the results were exported for comparison.  
[PowerShell: Guest Account Group Membership Remediation](https://github.com/joshmadakor1/lognpacific-public/blob/main/automation/toggle-guest-local-administrators.ps1)  

<img width="916" height="506" alt="image" src="https://github.com/user-attachments/assets/14df7685-be28-4227-9ca1-477cb18a7f10" />


[Scan 4 - Guest Account Group Removal](https://drive.google.com/file/d/1wGZfRxLCyiOWeep11TEQOq-eahz4FmRs/view?usp=sharing)


#### Remediation Round 4: Windows OS Updates

Windows updates were re-enabled and applied until the system was fully up to date. A final scan verified the changes  

<img width="916" height="489" alt="image" src="https://github.com/user-attachments/assets/e4123c65-4111-4e1a-b2c8-42316b64a32a" />



[Scan 5 - Post Windows Updates](https://drive.google.com/file/d/1e1AGudutGU67QrmQA6mag1IlGN-q30aA/view?usp=sharing)

---

### First Cycle Remediation Effort Summary

The remediation process reduced total vulnerabilities by 80%, from 30 to 6. Critical vulnerabilities were resolved by the second scan (100%), and high vulnerabilities dropped by 90%. Mediums were reduced by 76%. In an actual production environment, asset criticality would further guide future remediation efforts.  

<img width="1920" alt="image" src="https://github.com/user-attachments/assets/51f0aae8-7f36-4d90-b29f-5257e57155f9">

[Remediation Data](https://docs.google.com/spreadsheets/d/1BChtC0t45vmyGxzgJofW3lpPYpcuS8n4spFetoRb6uc/edit?usp=sharing)

---

### On-going Vulnerability Management (Maintenance Mode)

After completing the initial remediation cycle, the vulnerability management program transitions into **Maintenance Mode**. This phase ensures that vulnerabilities continue to be managed proactively, keeping systems secure over time. Regular scans, continuous monitoring, and timely remediation are crucial components of this phase. (See [Finalized Policy](https://docs.google.com/document/d/1lrR8cZ4zJnW9P8eKg7Vo6k4zY7GslSxo4wxsWGhqbXI/edit?usp=sharing) for scanning and remediation cadence requirements.)

Key activities in Maintenance Mode include:
- **Scheduled Vulnerability Scans**: Perform regular scans (e.g., weekly or monthly) to detect new vulnerabilities as systems evolve.
- **Patch Management**: Continuously apply security patches and updates, ensuring no critical vulnerabilities remain unpatched.
- **Remediation Follow-ups**: Address newly identified vulnerabilities promptly, prioritizing based on risk and impact.
- **Policy Review and Updates**: Periodically review the Vulnerability Management Policy to ensure it aligns with the latest security best practices and organizational needs.
- **Audit and Compliance**: Conduct internal audits to ensure compliance with the vulnerability management policy and external regulations.
- **Ongoing Communication with Stakeholders**: Maintain open communication with teams responsible for remediation, ensuring efficient coordination.

By maintaining an active vulnerability management process, organizations can stay ahead of emerging threats and ensure long-term security resilience.
- **Patch Management**: Continuously apply security patches and updates, ensuring no critical vulnerabilities remain unpatched.
- **Remediation Follow-ups**: Address newly identified vulnerabilities promptly, prioritizing based on risk and impact.
- **Policy Review and Updates**: Periodically review the Vulnerability Management Policy to ensure it aligns with the latest security best practices and organizational needs.
- **Audit and Compliance**: Conduct internal audits to ensure compliance with the vulnerability management policy and external regulations.
- **Ongoing Communication with Stakeholders**: Maintain open communication with teams responsible for remediation, ensuring efficient coordination.

By maintaining an active vulnerability management process, organizations can stay ahead of emerging threats and ensure long-term security resilience.
