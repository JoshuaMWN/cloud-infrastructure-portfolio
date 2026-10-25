[![AZ-305](https://img.shields.io/badge/Certification-AZ--305%20Expert-blue?style=for-the-badge)](https://learn.microsoft.com/api/credentials/share/en-us/JoshuaNeylan-1125/EB64157968252EA6?sharingId=33227CFBBD030D51)
[![AZ-104](https://img.shields.io/badge/Certification-AZ--104%20Associate-blue?style=for-the-badge)](https://learn.microsoft.com/api/credentials/share/en-us/JoshuaNeylan-1125/4DC3AFA2C4F5F232?sharingId=33227CFBBD030D51)
# cloud-infrastructure-portfolio
Hands-on Azure and Windows Server infrastructure labs covering identity, networking, monitoring, backup and governance, with documented troubleshooting.
## About Me
I am an IT Systems Specialist, Microsoft Certified (AZ-305, AZ-104), building my skills in systems and infrastructure administration: identity and access management, Windows Server, networking, monitoring, backup and PowerShell automation. This portfolio documents hands-on Azure labs, including the faults I hit and how I diagnosed them.

**Target Location:** Melbourne, VIC (Relocating December 2026)  
**Looking For:** Systems Administrator / Cloud Administrator / Systems Engineer  
**Core Tech Stack:** Azure Core Services (Compute, Networking, Storage, Governance), Entra ID, Azure Key Vault, Active Directory, PowerShell, Azure CLI, ARM Templates/IaC, Git/GitHub.  

---
## Featured Highlights: Systems Administration Skills Matrix

A map of core infrastructure skills to the labs that best demonstrate them. Each link goes straight to the relevant lab write-up. The full chronological index is shown further down.

| Skill area | Lab | What I did and diagnosed | Tools |
|---|---|---|---|
| **Windows Server, Active Directory, PKI** | [05 / Lab 4: AD CS and HTTPS binding](05-hybrid-identity-and-secrets/README.md#lab-4-active-directory-certificate-services-ad-cs-deployment-template-customization-and-https-binding) | Deployed an Enterprise Root CA on a domain controller, built a custom certificate template, diagnosed why it showed as *Unavailable* (not published on the CA), and bound a trusted certificate to IIS over HTTPS. | Windows Server, AD CS, IIS, MMC |
| **Entra ID and identity** | [05 / Lab 2: Entra SAML SSO](05-hybrid-identity-and-secrets/README.md#lab-2-enterprise-single-sign-on-sso-integration-via-microsoft-entra-saml-toolkit) | Configured SAML 2.0 SSO, traced a failed sign-in to mismatched instance-specific ACS/Reply URLs, and validated the end-to-end flow. | Entra ID, SAML, My Apps |
| **PowerShell and automation** | [04 / Lab 1: ARM, PowerShell and CLI provisioning](04-provisioning-automation-and-monitoring/README.md#lab-1-automated-infrastructure-provisioning-via-arm-templates-azure-powershell-and-azure-cli) | Deployed equivalent VMs three ways and debugged a policy-driven naming failure, a PowerShell parameter-type error and CLI flag casing. | ARM, Azure PowerShell, Azure CLI |
| **Monitoring and alerting** | [04 / Lab 2: Metric alerting](04-provisioning-automation-and-monitoring/README.md#lab-2-performance-monitoring-and-automated-metric-alerting) | Built an alert rule and action group, triggered it with a CPU stress test, and observed the full alert-to-resolution lifecycle, including evaluation delay. | Azure Monitor, Action Groups, Linux |
| **Troubleshooting across layers** | [04 / Lab 4: App diagnostics](04-provisioning-automation-and-monitoring/README.md#lab-4-azure-app-service-deployment-continuous-integration-apm-and-web-server-diagnostics) | Found why the app log stream was empty (404s handled by IIS before app code ran) and switched to web server logs to see the traffic. | App Service, Application Insights |
| **Networking and connectivity** | [01 / Lab 3: VNet peering](01-cloud-routing-and-isolation/README.md#lab-3-interconnecting-networks-via-vnet-peering) | Diagnosed 100% ping loss as the target OS dropping ICMP, then confirmed connectivity with a TCP port probe. | VNet peering, netcat, PowerShell |
| **Backup and SQL Server protection** | [03 / Lab 4: Azure Backup and SQL protection](03-deployment-balancing-and-protection/README.md#lab-4-protect-workloads-azure-backup-and-sql-workload-protection) | Configured VM backup policies and SQL workload discovery with transaction log backups for point-in-time recovery. | Recovery Services Vault, SQL Server |
| **Secrets and access control** | [05 / Lab 1: Key Vault and managed identity](05-hybrid-identity-and-secrets/README.md#lab-1-azure-key-vault-integration-managed-identity-authentication-and-secret-management) | Gave an app passwordless access to a secret using a managed identity and a least-privilege access policy. | Key Vault, Managed Identity |

### Full lab index (chronological)

1. [01 Cloud Routing and Isolation](01-cloud-routing-and-isolation)
2. [02 Storage Security and Governance](02-storage-security-and-governance)
3. [03 Deployment, Balancing and Protection](03-deployment-balancing-and-protection)
4. [04 Provisioning, Automation and Monitoring](04-provisioning-automation-and-monitoring)
5. [05 Hybrid Identity and Secrets](05-hybrid-identity-and-secrets)

---

## Certifications & Technical Milestones
**Microsoft Certified: Azure Solutions Architect Expert (AZ-305)**  
Status: Active (Issued October 2026)  
Verification: [View Microsoft Official Transcript](https://learn.microsoft.com/api/credentials/share/en-us/JoshuaNeylan-1125/EB64157968252EA6?sharingId=33227CFBBD030D51)

**Microsoft Certified: Azure Administrator Associate (AZ-104)**  
Status: Active (Issued August 2026)  
Verification: [View Microsoft Official Transcript](https://learn.microsoft.com/api/credentials/share/en-us/JoshuaNeylan-1125/4DC3AFA2C4F5F232?sharingId=33227CFBBD030D51)  