# hybrid-identity-and-secrets
## Project Focus
This repository documents practical implementation steps for enterprise identity federation, secret management, Public Key Infrastructure (PKI), and cloud platform deployment across Microsoft Azure and Active Directory environments. The core objective was strengthening authentication boundary security, operational agility, and system trustworthiness. This was achieved through securing workloads via Managed Identities and Key Vaults, setting up SAML SSO, automating TLS certificate bindings, and externalizing app configuration. These labs capture essential identity and platform administration across hybrid and cloud environments.  



## Lab Module Breakdowns
### Lab 1: Azure Key Vault Integration, Managed Identity Authentication, and Secret Management

**Objective:** Provision a secure Azure Key Vault instance, configure access control policies using system-assigned Managed Identity for passwordless authentication, store sensitive application configuration settings, and integrate secret retrieval directly into an Azure App Service application.  

**Technical Implementation:** Deployed a Standard Key Vault with Vault Access Policies, enabled system-assigned Managed Identity on an Azure App Service instance, granted secret Get and List permissions to the app's Service Principal, created a target secret, and configured Key Vault reference environment variables in App Service to safely load sensitive values without embedding plain-text credentials in application code.  

**To establish secure credential management, a Standard tier Azure Key Vault was provisioned with Vault access policies enabled:**  

![lab1-keyvault-creation-review](assets/lab1-keyvault-creation-review.png)  

**To enable passwordless authentication between Azure resources, system-assigned Managed Identity was enabled on the target App Service:**  

![lab1-appservice-managed-identity](assets/lab1-appservice-managed-identity.png)  

**An Access Policy was configured on the Key Vault targeting the App Service's Managed Identity principal, granting minimal required Secret Management permissions:**  

![lab1-keyvault-access-policy-permissions](assets/lab1-keyvault-access-policy-permissions.png)  

**Reviewing and creating the Access Policy confirmed proper binding between the Key Vault and the App Service principal without exposing broad administrative permissions:**  

![lab1-keyvault-access-policy-review](assets/lab1-keyvault-access-policy-review.png)  

**A new secret named connection2 was generated inside the Key Vault to store sensitive connection details:**  

![lab1-keyvault-create-secret](assets/lab1-keyvault-create-secret.png)  

**Auditing the Key Vault Secrets blade confirmed that connection2 was created and active:**  

![lab1-keyvault-secrets-list](assets/lab1-keyvault-secrets-list.png)  

**Within the App Service Environment variables, an app setting named secretUri was created, setting its value to the direct Key Vault secret URI:**  

![lab1-appservice-add-env-variable](assets/lab1-appservice-add-env-variable.png)  

**Reviewing the environment settings confirmed that the URI was mapped directly from the Key Vault source:**  

![lab1-appservice-env-variables-list](assets/lab1-appservice-env-variables-list.png)  

**Accessing the live web application and executing the "Test Key Vault" function successfully validated that the application retrieved secrets from Key Vault via Managed Identity:**  

![lab1-app-keyvault-validation](assets/lab1-app-keyvault-validation.png)  

**Key Takeaway:** Using Azure Key Vault combined with system-assigned Managed Identities eliminates the risk of hardcoding secrets in application source code or configuration files. Granting granular access policies ensures workloads adhere to the principle of least privilege while maintaining seamless secret retrieval across Azure environments.  


### Lab 2: Enterprise Single Sign-On (SSO) Integration via Microsoft Entra SAML Toolkit

**Objective:** Provision an enterprise SAML application within Microsoft Entra ID, configure linked SAML authentication attributes and certificates, troubleshoot route misconfigurations, and validate end-to-end single sign-on flows.  

**Technical Implementation:** Configured a SAML 2.0 enterprise application in Microsoft Entra ID, bound raw certificates and identity claims, resolved token delivery failures by updating Entra with instance-specific ACS dynamic endpoints, and validated end-to-end SSO authentication from the My Apps portal.  

**To provision the test application within the tenant, the Microsoft Entra SAML Toolkit was selected and added directly from the Enterprise App Gallery:**  

![lab2-entra-app-gallery-saml-toolkit](assets/lab2-entra-app-gallery-saml-toolkit.png)  

**Initial Basic SAML parameters were defined inside Microsoft Entra ID, pointing to the base Identifier and default placeholder endpoint URLs:**  

![lab2-entra-basic-saml-configuration](assets/lab2-entra-basic-saml-configuration.png)  

**To isolate access permissions and properly validate token generation during testing, a dedicated user account was created within the tenant:**  

![lab2-entra-create-user-basics](assets/lab2-entra-create-user-basics.png)  

**A quick review of the new account profile confirmed UPN formatting and assignment details:**  

![lab2-entra-create-user-review](assets/lab2-entra-create-user-review.png)  

**With the profile active, explicit access rights were assigned to the test user under the application's Users and groups blade:**  

![lab2-entra-assign-user-to-app](assets/lab2-entra-assign-user-to-app.png)  

**Next, the test user was registered within the SAML Toolkit application itself, matching the UPN configured in Entra ID:**  

![lab2-saml-toolkit-user-registration](assets/lab2-saml-toolkit-user-registration.png)  

**To establish the identity relationship, the SAML Toolkit application was configured with Entra's Login URL, Entity ID, and raw SAML Certificate:**  

![lab2-saml-toolkit-sso-configuration](assets/lab2-saml-toolkit-sso-configuration.png)  

**However, attempting to launch the application via the My Apps portal resulted in an incomplete flow, redirecting back to the unauthenticated SAML Toolkit home page:**  

![lab2-saml-toolkit-landing-page](assets/lab2-saml-toolkit-landing-page.png)  

**Investigation revealed that Entra ID was still targeting default base URLs "/SAML/Consume" and "/SAML/Login". Because the SAML Toolkit is a multi-tenant application, saving the configuration dynamically generates instance-specific endpoints that must be fed back into Entra ID. Once the SAML binding was saved in the app, these unique URLs incorporating connection ID "22139" became available:**  

![lab2-saml-toolkit-generated-urls](assets/lab2-saml-toolkit-generated-urls.png)  

**Returning to the SSO configuration in Entra ID, the Reply URL and Sign on URL fields were updated to match the generated dynamic endpoints:**  

![lab2-entra-updated-saml-urls](assets/lab2-entra-updated-saml-urls.png)  

**With the endpoints aligned, I logged into the My Apps portal using the test account and selected the application tile once again:**  

![lab2-myapps-launch-test-user](assets/lab2-myapps-launch-test-user.png)  

**The SAML assertion handshake completed successfully, granting full access and redirecting the user to the application dashboard with an active, verified session:**  

![lab2-sso-success-authenticated](assets/lab2-sso-success-authenticated.png)  

**Key Takeaway:** SAML centralizes authentication. Instead of each application managing its own user list and password policy, Entra ID becomes the authoritative identity source. From an operational perspective: when a new employee joins, they get one identity in Entra ID and automatically gain access to every provisioned application. When they leave, one account's decommissioning removes access everywhere. When password policy changes (eg: require 12-character passwords minimum), it applies to all applications instantly. In traditional environments, each app has its own user store and policies, synchronizing changes across dozens of applications is operationally expensive and error-prone.  


### Lab 3: Azure App Service Deployment, Continuous Integration, and Dynamic Environment Configuration  

**Objective:** Provision a dedicated Windows-based Azure App Service instance, establish continuous deployment via External Git, configure dynamic application settings through environment variables, and utilize the App Service Editor for in-browser file modification and troubleshooting.  

**Technical Implementation:** Deployed an S1 Standard App Service instance running ASP.NET v4.8, integrated dynamic Razor view bindings in Index.cshtml, bound Azure App Settings (AppTitle) to inject environment variables at runtime, and verified swapping configuration settings without application re-deployments.  


**To establish the web application hosting platform, an Azure App Service instance was provisioned under a Standard (S1) App Service Plan running ASP.NET v4.8:**  

![lab3-appservice-creation-review](assets/lab3-appservice-creation-review.png)  

**Once provisioned, navigating to the default hostname verified that the App Service instance was healthy and serving the standard Microsoft hosting placeholder page:**  

![lab3-appservice-default-landing](assets/lab3-appservice-default-landing.png)  

**Continuous deployment was configured within Deployment Center by connecting an external public GitHub repository containing the ASP.NET application source code:**  

![lab3-deployment-center-external-git](assets/lab3-deployment-center-external-git.png)  

**After allowing the build pipeline and deployment sync to finish, accessing the public endpoint confirmed that the web application was live and operational:**  

![lab3-aspnet-app-online](assets/lab3-aspnet-app-online.png)  

**Using the embedded App Service Editor, line 6 was updated to dynamically bind the header tag to the Azure App Setting using Razor syntax:**  

![lab3-app-editor-index-modified](assets/lab3-app-editor-index-modified.png)  

**During the initial view binding, navigating to the landing page briefly triggered a runtime error screen:**  

![lab3-app-error-processing-request](assets/lab3-app-error-processing-request.png)  

**Once saved and corrected in the App Service Editor:**  

```HTML
<h1>@System.ConfigurationManager.AppSettings[AppTitle]</h1> --> <h1>@System.Configuration.ConfigurationManager.AppSettings["AppTitle"]</h1>
```  

![lab3-app-editor-index-saved](assets/lab3-app-editor-index-saved.png)  

**Refreshing the live site confirmed that the application successfully loaded the dynamically injected string (This is my ASP.NET Web App):**  

![lab3-aspnet-app-initial-title](assets/lab3-aspnet-app-initial-title.png)  

**To verify that production updates can occur at the platform configuration layer without requiring code modifications or redeployments, the AppTitle variable was updated in the Azure Portal:**  

![lab3-env-variable-apptitle-updated](assets/lab3-env-variable-apptitle-updated.png)  

**Applying the configuration change and refreshing the web application instantly rendered the updated header (This is my new ASP.NET Web App) live on the site:**  

![lab3-aspnet-app-final-title](assets/lab3-aspnet-app-final-title.png)  

**Key Takeaway:** Separating application configuration from core codebases using App Service environment variables enables operational agility. Configuration changes deploy instantly with no risk of introducing bugs through code changes. This is critical for incident response. The separation created using environment variables provides crucial audit trails. When you change a setting, App Service logs what changed, by whom, and when. Configuration becomes auditable infrastructure, not scattered across source commits.


### Lab 4: Active Directory Certificate Services (AD CS) Deployment, Template Customization, and HTTPS Binding

**Objective:** Provision Active Directory Certificate Services (AD CS) on a Windows Server domain controller, configure an Enterprise Root Certificate Authority, publish custom web server certificate templates, resolve template availability enrollment issues, and secure local IIS web services over HTTPS.  

**Technical Implementation:** Deployed AD CS and IIS roles on DC01.hexelo.com, configured an Enterprise Root CA, created a duplicated Custom Web Server certificate template with explicit enrollment permissions, published the template to the CA, issued a local SSL certificate, and bound port 443 in IIS to establish trusted TLS communication.  

**To establish internal Public Key Infrastructure, the Active Directory Certificate Services role and associated Remote Server Administration Tools were selected in Server Manager:**  

![lab4-adcs-role-selection](assets/lab4-adcs-role-selection.png)  

**Required dependency features including .NET Framework 4.8 WCF Services and Web Server (IIS) management components were added to support enrollment services:**  

![lab4-iis-dependencies-selection](assets/lab4-iis-dependencies-selection.png)  

**During post-deployment configuration, the setup type was defined as an Enterprise CA to integrate directly with Active Directory Domain Services:**  

![lab4-adcs-enterprise-ca-setup](assets/lab4-adcs-enterprise-ca-setup.png)  

**The Enterprise Root CA parameters were reviewed and confirmed using RSA 2048-bit encryption with SHA256 hashing:**  

![lab4-adcs-confirmation-review](assets/lab4-adcs-confirmation-review.png)  

**Execution completed successfully, establishing DC01.hexelo.com as the primary active Enterprise Certification Authority:**  

![lab4-adcs-configuration-success](assets/lab4-adcs-configuration-success.png)  

**To support HTTPS web endpoints, a new template named Custom Web Server was duplicated from the standard Web Server template:**  

![lab4-template-general-properties](assets/lab4-template-general-properties.png)  

**Security permissions were updated to grant Authenticated Users explicit Read and Enroll rights:**  

![lab4-template-security-permissions](assets/lab4-template-security-permissions.png)  

**Subject Name requirements were configured to build dynamically from Active Directory domain information:**  

![lab4-template-subject-name-ad](assets/lab4-template-subject-name-ad.png)  

**An MMC management console was configured with the Certificates snap-in targeted at the Local Computer account to manage host certificates:**  

![lab4-mmc-certificates-snapin](assets/lab4-mmc-certificates-snapin.png)  

**When attempting initial certificate enrollment, the Custom Web Server template returned a status of Unavailable due to not yet being published on the Certification Authority:**  

![lab4-enrollment-template-unavailable](assets/lab4-enrollment-template-unavailable.png)  

**To fix this issue, the Certification Authority console was opened, and the Custom Web Server template was explicitly added:**  

![lab4-ca-enable-template](assets/lab4-ca-enable-template.png)  

**Re-running the Certificate Enrollment Wizard confirmed that the Custom Web Server template was now Available for request:**  

![lab4-enrollment-template-available](assets/lab4-enrollment-template-available.png)  

**The certificate request was submitted and completed with a status of Succeeded:**  

![lab4-enrollment-succeeded](assets/lab4-enrollment-succeeded.png)  

**Opening the Local Computer Personal store verified that the Web Server 1 certificate issued by hexelo-DC01-CA was installed and active:**  

![lab4-personal-store-verification](assets/lab4-personal-store-verification.png)  

**In IIS Manager, an HTTPS binding was added to the Default Web Site on port 443, attaching the newly issued Web Server 1 SSL certificate:**  

![lab4-iis-https-binding](assets/lab4-iis-https-binding.png)

**Navigating to https://dc01.hexelo.com in Microsoft Edge verified a secure, trusted SSL connection without certificate authority warnings:**  

![lab4-browser-https-validation](assets/lab4-browser-https-validation.png)  

**Key Takeaway:** Enterprise CA deployment provides centralized control over HTTPS across your infrastructure. Instead of buying certificates from external vendors (costly and operationally complex to track renewal dates), internal CAs issue certificates from your own root. From an operational perspective, certificate lifecycle becomes automatable. Renewal workflows, revocation procedures, and audit trails are all under your control.




