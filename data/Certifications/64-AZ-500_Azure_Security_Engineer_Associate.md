---
date: 09-08-22
id: 64
path: Certifications
tags: []
title: 'AZ-500: Azure Security Engineer Associate'
type: note
---

# AZ-500 : Azure Security Engineer Associate

https://docs.microsoft.com/en-us/certifications/azure-security-engineer/

# Learning Path: Manage Identity and Access

https://docs.microsoft.com/en-us/learn/paths/manage-identity-access/

# Module: Secure Azure solutions with Azure Active Directory

https://docs.microsoft.com/en-us/learn/modules/azure-active-directory/

## Explore Azure Active Directory features

Azure Active Directory (Azure AD) is Microsoft’s multi-tenant cloud-based directory and identity management service. For IT Admins, Azure AD provides an affordable, easy to use solution to give employees and business partners single sign-on (SSO) access to thousands of cloud SaaS Applications like Office365, Salesforce, DropBox, and Concur.

For application developers, Azure AD lets you focus on building your application by making it fast and simple to integrate with a world class identity management solution used by millions of organizations around the world.

### Identity manage capabilities and integration

Azure AD also includes a full suite of identity management capabilities including multifactor authentication, device registration, self-service password management, self-service group management, privileged account management, role-based access control, application usage monitoring, rich auditing and security monitoring, and alerting. These capabilities can help secure cloud-based applications, streamline IT processes, cut costs, and help assure corporate compliance goals are met.

Additionally, Azure AD can be integrated with an existing Windows Server Active Directory, giving organizations the ability to leverage their existing on-premises identity investments to manage access to cloud based SaaS applications.

### Azure AD Editions

Azure Active Directory comes in four editions—Free, Microsoft 365 Apps, Premium P1, and Premium P2. The Free edition is included with an Azure subscription. The Premium editions are available through a Microsoft Enterprise Agreement, the Open Volume License Program, and the Cloud Solution Providers program. Azure and Microsoft 365 subscribers can also buy Azure Active Directory Premium P1 and P2 online.

* Azure Active Directory Free – Provides user and group management, on-premises directory synchronization, basic reports, and single sign-on across Azure, Microsoft 365, and many popular SaaS apps.
* Azure Active Directory Microsoft 365 Apps - This edition is included with O365. In addition to the Free features, this edition provides Identity & Access Management for Microsoft 365 apps including branding, MFA, group access management, and self-service password reset for cloud users.
* Azure Active Directory Premium P1 - In addition to the Free features, P1 also lets your hybrid users access both on-premises and cloud resources. It also supports advanced administration, such as dynamic groups, self-service group management, Microsoft Identity Manager (an on-premises identity and access management suite) and cloud write-back capabilities, which allow self-service password reset for your on-premises users.
* Azure Active Directory Premium P2 - In addition to the Free and P1 features, P2 also offers Azure Active Directory Identity Protection to help provide risk-based Conditional Access to your apps and critical company data and Privileged Identity Management to help discover, restrict, and monitor administrators and their access to resources and to provide just-in-time access when needed.

The Azure Active Directory Pricing page has detailed information on what is included in each of the editions. Based on the feature list which edition does your organization need?

Note: If you are a Microsoft 365, Azure or Dynamics CRM Online customer, you might not realize that you are already using Azure AD. Every Microsoft 365, Azure and Dynamics CRM tenant are already an Azure AD tenant. Whenever you want you can start using that tenant to manage access to thousands of other cloud applications Azure AD integrates with.

## Compare Azure AD vs Active Directory Domain Services

**Azure AD is different from AD DS** - Although Azure AD has many similarities to AD DS, there are also many differences. It is important to realize that using Azure AD is different from deploying an Active Directory domain controller on an Azure virtual machine and adding it to your on-premises domain. Here are some characteristics of Azure AD that make it different.

* Identity solution. Azure AD is primarily an identity solution, and it is designed for Internet-based applications by using HTTP and HTTPS communications.
* REST API Querying. Because Azure AD is HTTP/HTTPS based, it cannot be queried through LDAP. Instead, Azure AD uses the REST API over HTTP and HTTPS.
* Communication Protocols. Because Azure AD is HTTP/HTTPS based, it does not use Kerberos authentication. Instead, it uses HTTP and HTTPS protocols such as SAML, WS-Federation, and OpenID Connect for authentication (and OAuth for authorization).
* Authentication Services. Include SAML, WS-Federation, or OpenID.
* Authorization Service. Uses OAuth.
* Federation Services. Azure AD includes federation services, and many third-party services (such as Facebook).
* Flat structure. Azure AD users and groups are created in a flat structure, and there are no Organizational Units (OUs) or Group Policy Objects (GPOs).

## Investigate roles in Azure AD

Using Azure Active Directory (Azure AD), you can designate limited administrators to manage identity tasks in less-privileged roles. Administrators can be assigned for such purposes as adding or changing users, assigning administrative roles, resetting user passwords, managing user licenses, and managing domain names. The default user permissions can be changed only in user settings in Azure AD.

### Limit the use of Global administrators

Users who are assigned to the Global administrator role can read and modify every administrative setting in your Azure AD organization. By default, the person who signs up for an Azure subscription is assigned the Global administrator role for the Azure AD organization. Only Global administrators and Privileged Role administrators can delegate administrator roles. To reduce the risk to your business, we recommend that you assign this role to the fewest possible people in your organization.

As a best practice, we recommend that you assign this role to fewer than five people in your organization. If you have more than five admins assigned to the Global Administrator role in your organization, here are some ways to reduce its use.

### Available roles

* Application Administrator - Users in this role can create and manage all aspects of enterprise applications, application registrations, and application proxy settings.
* Application Developer - Users in this role can create application registrations when the "Users can register applications" setting is set to Yes.
* Authentication Administrator - Users with this role can set or reset any authentication method (including passwords) for non-administrators and some roles. Authentication Administrators can require users who are non-administrators or assigned to some roles to re-register against existing non-password credentials (for example, multifactor authentication or Fast Identity Online), and can also revoke remember multi-factor authentication on the device, which prompts for multi-factor authentication on the next sign-in.
* Azure DevOps Administrator - Users with this role can manage the Azure DevOps policy to restrict new Azure DevOps organization creation to a set of configurable users or groups.
* Azure Information Protection Administrator - Users with this role have all permissions in the Azure Information Protection service.
* B2C User Flow Administrator - Users with this role can create and manage B2C User Flows (also called "built-in" policies) in the Azure portal.
* B2C User Flow Attribute Administrator - Users with this role add or delete custom attributes available to all user flows in the tenant.
* B2C IEF Keyset Administrator - User can create and manage policy keys and secrets for token encryption, token signatures, and claim encryption/decryption.
* B2C IEF Policy Administrator - Users in this role can create, read, update, and delete all custom policies in Azure AD B2C and therefore have full control over the Identity Experience Framework in the relevant Azure AD B2C tenant.
* Billing Administrator - Makes purchases, manages subscriptions, manages support tickets, and monitors service health.
* Cloud Application Administrator - Users in this role have the same permissions as the Application Administrator role, excluding the ability to manage application proxy.
* Cloud Device Administrator - Users in this role can enable, disable, and delete devices in Azure AD and read Windows 10 BitLocker keys (if present) in the Azure portal.
* Compliance Administrator - Users with this role have permissions to manage compliance-related features in the Microsoft 365 compliance portal, Microsoft 365 admin center, Azure, and Microsoft 365 Security & compliance portal.
* Compliance Data Administrator - Users with this role have permissions to track data in the Microsoft 365 compliance portal, Microsoft 365 admin center, and Azure. Users can also track compliance data within the Exchange admin center,
* Conditional Access Administrator - Users with this role have the ability to manage Azure Active Directory Conditional Access settings
* Exchange Administrator - Users with this role have global permissions within Microsoft Exchange Online when the service is present.
* Directory Readers - Users in this role can read basic directory information.
* Global Administrator / Company Administrator - Users with this role have access to all administrative features in Azure Active Directory, as well as services that use Azure Active Directory identities like Microsoft 365 security center, Microsoft 365 compliance portal, Exchange Online, SharePoint Online, and Skype for Business Online.
* Groups Administrator - Users in this role can create/manage groups and its settings like naming and expiration policies.
* Security Administrator - Users with this role have permissions to manage security-related features in the Microsoft 365 security center, Azure Active Directory Identity Protection, Azure Information Protection, and Microsoft 365 Security & compliance portal.

For most organizations, the security of business assets depends on the integrity of the privileged accounts that administer and manage IT systems. Cyber-attackers focus on privileged access to infrastructure systems (such as Active Directory and Azure Active Directory) to gain access to an organization’s sensitive data.

Traditional approaches that focus on securing the entrance and exit points of a network as the primary security perimeter are less effective due to the rise in the use of SaaS apps and personal devices on the Internet. The natural replacement for the network security perimeter in a complex modern enterprise is the authentication and authorization controls in an organization's identity layer.

Privileged administrative accounts are effectively in control of this new security perimeter. It's critical to protect privileged access, regardless of whether the environment is on-premises, cloud, or hybrid on-premises and cloud-hosted services. Protecting administrative access against determined adversaries requires you to take a complete and thoughtful approach to isolating your organization’s systems from risks.

## Deploy Azure AD Domain Services

Azure Active Directory Domain Services (Azure AD DS) provides managed domain services such as domain join, group policy, lightweight directory access protocol (LDAP), and Kerberos / NTLM authentication that is fully compatible with Windows Server Active Directory. You use these domain services without the need to deploy, manage, and patch domain controllers in the cloud. Azure AD DS integrates with your existing Azure AD tenant, which makes it possible for users to sign in using their existing credentials. You can also use existing groups and user accounts to secure access to resources, which provides a smoother lift-and-shift of on-premises resources to Azure.

Azure AD DS replicates identity information from Azure AD, so it works with Azure AD tenants that are cloud-only, or synchronized with an on-premises Active Directory Domain Services (AD DS) environment. The same set of Azure AD DS features exist for both environments.

* If you have an existing on-premises AD DS environment, you can synchronize user account information to provide a consistent identity for users.
* For cloud-only environments, you don't need a traditional on-premises AD DS environment to use the centralized identity services of Azure AD DS.

Azure Active Directory uses Azure AD DS to manage workloads and apps in Azure.

### Azure AD DS features and benefits

To provide identity services to applications and VMs in the cloud, Azure AD DS is fully compatible with a traditional AD DS environment for operations such as domain-join, secure LDAP (LDAPS), Group Policy and DNS management, and LDAP bind and read support. LDAP write support is available for objects created in the Azure AD DS managed domain, but not resources synchronized from Azure AD. The following features of Azure AD DS simplify deployment and management operations:

* Simplified deployment experience: Azure AD DS is enabled for your Azure AD tenant using a single wizard in the Azure portal.
* Integrated with Azure AD: User accounts, group memberships, and credentials are automatically available from your Azure AD tenant. New users, groups, or changes to attributes from your Azure AD tenant or your on-premises AD DS environment are automatically synchronized to Azure AD DS.
* Use your corporate credentials/passwords: Passwords for users in Azure AD DS are the same as in your Azure AD tenant. Users can use their corporate credentials to domain-join machines, sign in interactively or over remote desktop, and authenticate against the Azure AD DS managed domain.
* NTLM and Kerberos authentication: With support for NTLM and Kerberos authentication, you can deploy applications that rely on Windows-integrated authentication.
* High availability: Azure AD DS includes multiple domain controllers, which provide high availability for your managed domain. This high availability guarantees service uptime and resilience to failures.

In regions that support Azure Availability Zones, these domain controllers are also distributed across zones for additional resiliency.

Important: Azure AD DS integrates with Azure AD, which itself can synchronize with an on-premises AD DS environment. This ability extends central identity use cases to traditional web applications that run in Azure as part of a lift-and-shift strategy.

## Create and manage Azure AD users

In Azure AD, every user who needs access to resources needs a user account. A user account is a synced Active Directory Domain Services (AD DS) object or an Azure AD user object that contains all the information needed to authenticate and authorize the user during the sign-on process and to build the user's access token.

To view the Azure AD users, access the All users blade. Take a minute to access the portal and view your users. Notice the USER TYPE and SOURCE columns, as the following figure depicts.

Screenshot that depicts the All users blade, with the USER TYPE and SOURCE columns noted.

Typically, Azure AD defines users in three ways:

* Cloud identities - These users exist only in Azure AD. Examples are administrator accounts and users that you manage yourself. Their source is Azure AD.
* Directory-synchronized identities - These users exist in on-premises Active Directory. A synchronization activity that occurs via Azure AD Connect brings these users in to Azure.
* Guest users - These users exist outside Azure. Examples are accounts from other cloud providers and Microsoft accounts.

## Manage users with Azure AD groups

Azure AD allows you to define two different types of groups.

* Security groups. These are the most common and are used to manage member and computer access to shared resources for a group of users. For example, you can create a security group for a specific security policy. By doing it this way, you can give a set of permissions to all the members at once, instead of having to add permissions to each member individually. This option requires an Azure AD administrator.
* Microsoft 365 groups. These groups provide collaboration opportunities by giving members access to a shared mailbox, calendar, files, SharePoint site, and more. This option also lets you give people outside of your organization access to the group. This option is available to users as well as admins.


There are different ways you can assign group access rights:

* Assigned. Lets you add specific users to be members of this group and to have unique permissions.
* Dynamic User. Lets you use dynamic membership rules to automatically add and remove members. If a member's attributes change, the system reviews your dynamic group rules for the directory to determine if the member meets the rule requirements (is added) or no longer meets the rules requirements (is removed).
* Dynamic Device (Security groups only). Lets you use dynamic group rules to automatically add and remove devices. If a device's attributes change, the system reviews your dynamic group rules for the directory to determine if the device meets the rule requirements (is added) or no longer meets the rules requirements (is removed).

Important: Have you given any thought to which groups you need to create? Would you directly assign or dynamically assign membership?

## Configure Azure AD administrative units

An administrative unit is an Azure AD resource that can be a container for other Azure AD resources. An administrative unit can contain only users and groups. Administrative units restrict permissions in a role to any portion of your organization that you define. You could, for example, use administrative units to delegate the Helpdesk Administrator role to regional support specialists, so they can manage users only in the region that they support.

Note: To use administrative units, you need an Azure Active Directory Premium license for each administrative unit admin, and Azure Active Directory Free licenses for administrative unit members.

### Available roles

|----|----|
|Role|Description|
|----|----|
|Authentication Administrator|Has access to view, set, and reset authentication method information for any non-admin user in the assigned administrative unit only.|
|Groups Administrator|Can manage all aspects of groups and groups settings, such as naming and expiration policies, in the assigned administrative unit only.|
|Helpdesk Administrator|Can reset passwords for non-administrators and Helpdesk administrators in the assigned administrative unit only.|
|License Administrator|Can assign, remove, and update license assignments within the administrative unit only.|
|Password Administrator|Can reset passwords for non-administrators and Password Administrators within the assigned administrative unit only.|
|User Administrator|Can manage all aspects of users and groups, including resetting passwords for limited admins within the assigned administrative unit only.|

## Implement passwordless authentication

Sign-in without ever using a password. With passwordless, the password is replaced with something you have plus something you are or something you know. For example, Windows Hello for Business can use a biometric gesture like a face or fingerprint, or a device-specific PIN that isn't transmitted over a network.

### Passwordless authentication methods

* Windows Hello for Business - biometric and PIN credentials are directly tied to the user's PC, which prevents access from anyone other than the owner.
* FIDO2 Security Keys - generally stored on a USB stick, FIDO2 security keys are an unphishable standards-based passwordless authentication method that can come in any form factor.
* Microsoft Authenticator App - Authenticator App turns any iOS or Android phone into a strong, passwordless credential. Users can sign in to any platform or browser by getting a notification to their phone, matching a number displayed on the screen to the one on their phone, and then using their biometric (touch or face) or PIN to confirm.
* FIDO2 Smartcards (preview) - new method to use FIDO2 keys for passwordless login via a smartcard.
* Temporary Access Pass (preview) - time-limited passcode allows you to set up security keys and the Microsoft Authenticator without ever needing to use, much less know, your password!

### Benefits of passwordless authentication

* Increased security - Reduce the risk of phishing and password spray attacks by removing passwords as an attack surface.
* Better user experience - Give users a convenient way to access data from anywhere. Provide easy access to applications and services such as Outlook, OneDrive, or Office while mobile.
* Robust insights - Gain insights into users passwordless activity with robust logging and auditing.

# Module: Implement Hybrid Identity

https://docs.microsoft.com/en-us/learn/modules/hybrid-identity/

## Deploy Azure AD connect

Azure AD Connect will integrate your on-premises directories with Azure Active Directory. This allows you to provide a common identity for your users for Microsoft 365, Azure, and SaaS applications integrated with Azure AD.

Azure AD Connect provides the following features:

* Password hash synchronization. A sign-in method that synchronizes a hash of a users on-premises AD password with Azure AD.
* Pass-through authentication. A sign-in method that allows users to use the same password on-premises and in the cloud, but doesn't require the additional infrastructure of a federated environment.
* Federation integration. Federation is an optional part of Azure AD Connect and can be used to configure a hybrid environment using an on-premises AD FS infrastructure. It also provides AD FS management capabilities such as certificate renewal and additional AD FS server deployments.
* Synchronization. Responsible for creating users, groups, and other objects. As well as, making sure identity information for your on-premises users and groups is matching the cloud. This synchronization also includes password hashes.
* Health Monitoring. Azure AD Connect Health can provide robust monitoring and provide a central location in the Azure portal to view this activity.

When you integrate your on-premises directories with Azure AD, your users are more productive because there's a common identity to access both cloud and on-premises resources. However, this integration creates the challenge of ensuring that this environment is healthy so that users can reliably access resources both on premises and in the cloud from any device.

Azure Active Directory (Azure AD) Connect Health provides robust monitoring of your on-premises identity infrastructure. It enables you to maintain a reliable connection to Microsoft 365 and Microsoft Online Services. This reliability is achieved by providing monitoring capabilities for your key identity components. Also, it makes the key data points about these components easily accessible. Azure AD Connect Health helps you:

* Monitor and gain insights into AD FS servers, Azure AD Connect, and AD domain controllers.
* Monitor and gain insights into the synchronizations that occur between your on-premises AD DS and Azure AD.
* Monitor and gain insights into your on-premises identity infrastructure that is used to access Microsoft 365 or other Azure AD applications

With Azure AD Connect the key data you need is easily accessible. You can view and act on alerts, setup email notifications for critical alerts, and view performance data.

Important: using AD Connect Health works by installing an agent on each of your on-premises sync servers.

## Explore authentication options

Choosing an Azure AD Authentication method is important as it is one of the first important decisions when moving to the cloud as it will be the foundation of your cloud environment and is difficult to change at a later date.

You can choose cloud authentication which includes: Azure AD password hash synchronization and Azure AD Pass-through Authentication. You can also choose federated authentication where Azure AD hands off the authentication process to a separate trusted authentication system, such as on-premises Active Directory Federation Services (AD FS), to validate the user’s password.

### Summary

* Do you need on-premises Active Directory integration? If the answer is No, then you would use Cloud-Only authentication.
* If you do need on-premises Active Directory integration, then do you need to use cloud authentication, password protection, and your authentication requirements are natively supported by Azure AD? If the answer is Yes, then you would use Password Hash Sync + Seamless SSO.
* If you do need on-premises Active Directory integration, but you do not need to use cloud authentication, password protection, and your authentication requirements are natively supported by Azure AD, then you would use Pass-through Authentication Seamless SSO.
* If you need on-premises Active Directory integration, have an existing federation provider and your authentication requirements are NOT natively supported by Azure AD, then you would use Federation authentication.

## Configure Password Hash Synchronization (PHS)

The probability that you're blocked from getting your work done due to a forgotten password is related to the number of different passwords you need to remember. The more passwords you need to remember, the higher the probability to forget one. Questions and calls about password resets and other password-related issues demand the most helpdesk resources.

Password hash synchronization (PHS) is a feature used to synchronize user passwords from an on-premises Active Directory instance to a cloud-based Azure AD instance. Use this feature to sign in to Azure AD services like Microsoft 365, Microsoft Intune, CRM Online, and Azure Active Directory Domain Services (Azure AD DS). You sign in to the service by using the same password you use to sign in to your on-premises Active Directory instance. Password hash synchronization helps you to:

* Improve the productivity of your users.
* Reduce your helpdesk costs.

### How does this work?

In the background, the password synchronization component takes the user’s password hash from on-premises Active Directory, encrypts it, and passes it as a string to Azure. Azure decrypts the encrypted hash and stores the password hash as a user attribute in Azure AD.

When the user signs in to an Azure service, the sign-in challenge dialog box generates a hash of the user’s password and passes that hash back to Azure. Azure then compares the hash with the one in that user’s account. If the two hashes match, then the two passwords must also match and the user receives access to the resource. The dialog box provides the facility to save the credentials so that the next time the user accesses the Azure resource, the user will not be prompted.

Important: it is important to understand that this is same sign-in, not single sign-on. The user still authenticates against two separate directory services, albeit with the same user name and password. This solution provides a simple alternative to an AD FS implementation.

## Implement Pass-through Authentication (PTA)

Azure AD Pass-through Authentication (PTA) is an alternative to Azure AD Password Hash Synchronization, and provides the same benefit of cloud authentication to organizations. PTA allows users to sign in to both on-premises and cloud-based applications using the same user account and passwords. When users sign-in using Azure AD, Pass-through authentication validates the users’ passwords directly against an organization's on-premise Active Directory.

### Feature benefits

* Supports user sign-in into all web browser-based applications and into Microsoft Office client applications that use modern authentication.
* Sign-in usernames can be either the on-premises default username (userPrincipalName) or another attribute configured in Azure AD Connect (known as Alternate ID).
* Works seamlessly with conditional access features such as Azure Active Directory Multi-Factor Authentication to help secure your users.
* Integrated with cloud-based self-service password management, including password writeback to on-premises Active Directory and password protection by banning commonly used passwords.
* Multi-forest environments are supported if there are forest trusts between your AD forests and if name suffix routing is correctly configured.
* PTA is a free feature, and you don't need any paid editions of Azure AD to use it.
* PTA can be enabled via Azure AD Connect.
* PTA uses a lightweight on-premises agent that listens for and responds to password validation requests.
* Installing multiple agents provides high availability of sign-in requests.
* PTA protects your on-premises accounts against brute force password attacks in the cloud.

Important: this feature can be configured without using a federation service so that any organization, regardless of size, can implement a hybrid identity solution. Pass-through authentication is not only for user sign-in but allows an organization to use other Azure AD features, such as password management, role-based access control, published applications, and conditional access policies.

## Deploy Federation with Azure AD

Federation is a collection of domains that have established trust. The level of trust may vary, but typically includes authentication and almost always includes authorization. A typical federation might include a number of organizations that have established trust for shared access to a set of resources.

You can federate your on-premises environment with Azure AD and use this federation for authentication and authorization. This sign-in method ensures that all user authentication occurs on-premises. This method allows administrators to implement more rigorous levels of access control.

Important: if you decide to use Federation with Active Directory Federation Services (AD FS), you can optionally set up password hash synchronization as a backup in case your AD FS infrastructure fails.

## Explore the authentication decision tree

Choosing the correct authentication method is the first concern for organizations wanting to move their apps to the cloud. Don't take this decision lightly, for the following reasons:

* It's the first decision for an organization that wants to move to the cloud.
* The authentication method is a critical component of an organization’s presence in the cloud. It controls access to all cloud data and resources.
* It's the foundation of all the other advanced security and user experience features in Azure AD.

Identity is the new control plane of IT security, so authentication is an organization’s access guard to the new cloud world. Organizations need an identity control plane that strengthens their security and keeps their cloud apps safe from intruders.

Authentication methods

**Cloud Authentication** - When you choose this authentication method, Azure AD handles users' sign-in process. Coupled with seamless single sign-on (SSO), users can sign in to cloud apps without having to reenter their credentials. With cloud authentication, you can choose from two options:

* Azure AD password hash Synchronization
* Azure AD Pass-through Authentication

**Federated Authentication** - When you choose this authentication method, Azure AD hands off the authentication process to a separate trusted authentication system, such as on-premises Active Directory Federation Services (AD FS), to validate the user’s password. The authentication system can provide additional advanced authentication requirements. Examples are smartcard-based authentication or third-party multifactor authentication.

### Decision tree

Authentication decision tree described in the text.

Details on decision questions:

1. Azure AD can handle sign-in for users without relying on on-premises components to verify passwords.
1. Azure AD can hand off user sign-in to a trusted authentication provider such as Microsoft’s AD FS.
1. If you need to apply user-level Active Directory security policies such as account expired, disabled account, password expired, account locked out, and sign-in hours on each user sign-in, Azure AD requires some on-premises components.
1. Sign-in features not natively supported by Azure AD:
    * Sign-in using smartcards or certificates.
    * Sign-in using on-premises MFA Server.
    * Sign-in using third-party authentication solution.
    * Multi-site on-premises authentication solution.
1. Azure AD Identity Protection requires Password Hash Sync regardless of which sign-in method you choose, to provide the Users with leaked credentials report. Organizations can fail over to Password Hash Sync if their primary sign-in method fails and it was configured before the failure event.

Important: this decision tree is intended as a starting point to understand your options, but there can be others or even combinations of different options. For example, you can use Azure AD B2C and configure it to allow user sign-in for multi-tenant Azure AD tenants - with or without the traditional support for self-service sign up and social identity providers.

## Configure password writeback

Having a cloud-based password reset utility is great but most companies still have an on-premises directory where their users exist. How does Microsoft support keeping traditional on-premises Active Directory Domain Services (AD DS) in sync with password changes in the cloud?

Password writeback is a feature enabled with Azure AD Connect that allows password changes in the cloud to be written back to an existing on-premises directory in real time.

Password writeback provides:

* Enforcement of on-premises Active Directory Domain Services password policies. When a user resets their password, it is checked to ensure it meets your on-premises Active Directory Domain Services policy before committing it to that directory. This review includes checking the history, complexity, age, password filters, and any other password restrictions that you have defined in local Active Directory Domain Services.
* Zero-delay feedback. Password writeback is a synchronous operation. Your users are notified immediately if their password did not meet the policy or could not be reset or changed for any reason.
* Supports password changes from the access panel and Microsoft 365. When federated or password hash synchronized users come to change their expired or non-expired passwords, those passwords are written back to your local Active Directory Domain Services environment.
* Supports password writeback when an admin resets them from the Azure portal. Whenever an admin resets a user’s password in the Azure portal, if that user is federated or password hash synchronized, the password is written back to on-premises. This functionality is currently not supported in the Office admin portal.
* Doesn’t require any inbound firewall rules. Password writeback uses an Azure Service Bus relay as an underlying communication channel. All communication is outbound over port 443.

Important: to use self-service password reset (SSPR) you must have already configured Azure AD Connect in your environment.

# Deploy Azure AD identity protection

https://docs.microsoft.com/en-us/learn/modules/azure-ad-identity-protection/

## Explore Azure AD identity protection

**Identity Protection** is a tool that allows organizations to accomplish three key tasks:

* Automate the detection and remediation of identity-based risks.
* Investigate risks using data in the portal.
* Export risk detection data to third-party utilities for further analysis.

Identity Protection uses the learnings Microsoft has acquired from their position in organizations with Azure AD, the consumer space with Microsoft Accounts, and in gaming with Xbox to protect your users. Microsoft analysis 6.5 trillion signals per day to identify and protect customers from threats.

Risk detections in Azure AD Identity Protection include any identified suspicious actions related to user accounts in the directory. The signals generated that are fed to Identity Protection, can be further fed into tools like Conditional Access to make access decisions, or fed back to a security information and event management (SIEM) tool for further investigation based on your organization's enforced policies.

Identity Protection provides organizations access to powerful resources so they can quickly respond to suspicious activities.

### Identity Protection policies

Azure Active Directory Identity Protection includes three default policies that administrators can choose to enable. These policies include limited customization but are applicable to most organizations. All the policies allow for excluding users such as your emergency access or break-glass administrator accounts.

### Azure MFA registration policy

Identity Protection can help organizations roll out Azure Multi-Factor Authentication (MFA) using a Conditional Access policy requiring registration at sign-in. Enabling this policy is a great way to ensure new users in your organization have registered for MFA on their first day. Multi-factor authentication is one of the self-remediation methods for risk events within Identity Protection. Self-remediation allows your users to act on their own to reduce helpdesk call volume.

### Sign-in risk policy

Identity Protection analyzes signals from each sign-in, both real-time and offline, and calculates a risk score based on the probability that the sign-in wasn't performed by the user. Administrators can decide based on this risk score signal to enforce organizational requirements. Administrators can choose to block access, allow access, or allow access but require multi-factor authentication.

If risk is detected, users can perform multi-factor authentication to self-remediate and close the risky sign-in event to prevent unnecessary noise for administrators.

### Custom Conditional Access policy

Administrators can also choose to create a custom Conditional Access policy including sign-in risk as an assignment condition.

## Configure risk event detections

To protect your users, you can configure risk-based policies in Azure Active Directory (Azure AD) that automatically respond to risky behaviors. Azure AD Identity Protection policies can automatically block a sign-in attempt or require additional action, such as requiring a password change or prompt for Azure AD Multi-Factor Authentication. These policies work with existing Azure AD Conditional Access policies as an extra layer of protection for your organization. Users may never trigger a risky behavior in one of these policies, but your organization is protected if an attempt to compromise your security is made.

Each day, Microsoft collects and analyses trillions of anonymized signals as part of user sign-in attempts. These signals help build patterns of good user sign-in behavior and identify potential risky sign-in attempts. Azure AD Identity Protection can review user sign-in attempts and take additional action if there's suspicious behavior:

Some of the following actions may trigger Azure AD Identity Protection risk detection:

* Users with leaked credentials.
* Sign-ins from anonymous IP addresses.
* Impossible travel to atypical locations.
* Sign-ins from infected devices.
* Sign-ins from IP addresses with suspicious activity.

The following three policies are available in Azure AD Identity Protection to protect users and respond to suspicious activity. You can choose to turn the policy enforcement on or off, select users or groups for the policy to apply to, and decide if you want to block access at sign-in or prompt for additional action.

The insight you get for a detected risk detection is tied to your Azure AD subscription.

* User risk policy - Identifies and responds to user accounts that may have compromised credentials. Can prompt the user to create a new password.
* Sign-in risk policy - Identifies and responds to suspicious sign-in attempts. Can prompt the user to provide additional forms of verification using Azure AD Multi-Factor Authentication.
* MFA registration policy - Makes sure users are registered for Azure AD Multi-Factor Authentication. If a sign-in risk policy prompts for MFA, the user must already be registered for Azure AD Multi-Factor Authentication.

When you enable a policy user or sign-in risk policy, you can also choose the threshold for risk level - low and above, medium and above, or high. This flexibility lets you decide how aggressive you want to be in enforcing any controls for suspicious sign-in events.

## Implement user risk policy

Identity Protection can calculate what it believes is normal for a user's behavior and use that to base decisions for their risk. User risk is a calculation of probability that an identity has been compromised. Administrators can decide based on this risk score signal to enforce organizational requirements. Administrators can choose to block access, allow access, or allow access but require a password change using Azure AD self-service password reset.

A user risk policy shows different risk levels and the ability to block access.

The above image shows the configuration of User Risk Policy applied

* To user sign-ins
* Automatically respond based on a specific user’s risk level
* Provide the condition (risk level) and action (block or allow)
* Use a high threshold during policy roll out
* Use a low threshold for greater security

### Risky users

With the information provided by the risky users report, administrators can find:

* Which users are at risk, have had risk remediated, or have had risk dismissed?
* Details about detections
* History of all risky sign-ins
* Risk history

Administrators can then choose to act on these events. Administrators can choose to:

* Reset the user password
* Confirm user compromise
* Dismiss user risk
* Block user from signing in
* Investigate further using Azure ATP

## Implement sign-in risk policy

Sign-in risk represents the probability that a given authentication request isn't authorized by the identity owner. For users of Azure Identity Protection, sign-in risk can be evaluated as part of a Conditional Access policy. Sign-in Risk Policy supports the following conditions:

### Location

When configuring location as a condition, organizations can choose to include or exclude locations. These named locations may include the public IPv4 network information, country or region, or even unknown areas that don't map to specific countries or regions. Only IP ranges can be marked as a trusted location. When including any location, this option includes any IP address on the internet not just configured named locations. When selecting any location, administrators can choose to exclude all trusted or selected locations.

### Client apps

Conditional Access policies by default apply to browser-based applications and applications that utilize modern authentication protocols. In addition to these applications, administrators can choose to include Exchange ActiveSync clients and other clients that utilize legacy protocols.

* Browser - These include web-based applications that use protocols like SAML, WS-Federation, OpenID Connect, or services registered as an OAuth confidential client.
* Mobile apps and desktop clients - These access policies are commonly used when requiring a managed device, blocking legacy authentication, and blocking web applications but allowing mobile or desktop app.

### Risky sign-ins

The risky sign-ins report contains filterable data for up to the past 30 days (1 month).

With the information provided by the risky sign-ins report, administrators can find:

* Which sign-ins are classified as at risk, confirmed compromised, confirmed safe, dismissed, or remediated.
* Real-time and aggregate risk levels associated with sign-in attempts.
* Detection types triggered
* Conditional Access policies applied
* MFA details
* Device information
* Application information
* Location information

Administrators can then choose to take action on these events. Administrators can choose to:

* Confirm sign-in compromise
* Confirm sign-in safe

## Deploy multifactor authentication in Azure

Azure Active Directory Multi-Factor Authentication (MFA) helps safeguard access to data and applications while maintaining simplicity for users. It provides additional security by requiring a second form of authentication and delivers strong authentication through a range of easy to use authentication methods.

For organizations that need to be compliant with industry standards, such as the Payment Card Industry (PCI) Data Security Standard (DSS) version 3.2, MFA is a must have capability to authenticate users. Beyond being compliant with industry standards, enforcing MFA to authenticate users can also help organizations to mitigate credential theft attacks.

MFA is monitoring signals and allowing or denying access to resources.

The security of MFA two-step verification lies in its layered approach. Compromising multiple authentication factors presents a significant challenge for attackers. Even if an attacker manages to learn the user's password, it is useless without also having possession of the additional authentication method. Authentication methods include:

* Something you know (typically a password)
* Something you have (a trusted device that is not easily duplicated, like a phone)
* Something you are (biometrics)

### MFA Features

* Get more security with less complexity. Azure MFA helps safeguard access to data and applications and helps to meet customer demand for a simple sign-in process. Get strong authentication with a range of easy verification options—phone call, text message, or mobile app notification—and allow customers to choose the method they prefer.
* Mitigate threats with real-time monitoring and alerts. MFA helps protect your business with security monitoring and machine-learning-based reports that identify inconsistent sign-in patterns. To help mitigate potential threats, real-time alerts notify your IT department of suspicious account credentials.
* Use with Microsoft 365, Salesforce, and more. MFA for Microsoft 365 helps secure access to Microsoft 365 applications at no additional cost. Multifactor authentication is also available with Azure Active Directory Premium and thousands of software-as-a-service (SaaS) applications, including Salesforce, Dropbox, and other popular services.
* Add protection for Azure administrator accounts. MFA adds a layer of security to your Azure administrator account at no additional cost. When it's turned on, you need to confirm your identity to create a virtual machine, manage storage, or use other Azure services.

### MFA authentication options

Screenshot of MFA users settings page with options to allow or disallow users to create app passwords to sign in to non-browser apps. Also different verification options that can be selected by users, and a checkbox to enable users to remember MFA on devices they trust.

|Method|Description|
|----|----|
|Call to phone|Places an automated voice call. The user answers the call and presses # in the phone keypad to authenticate. The phone number is not synchronized to on-premises Active Directory. A voice call to phone is important because it persists through a phone handset upgrade, allowing the user to register the mobile app on the new device.|
|Text message to phone|Sends a text message that contains a verification code. The user is prompted to enter the verification code into the sign-in interface. This process is called one-way SMS. Two-way SMS means that the user must text back a particular code. Two-way SMS is deprecated and not supported after November 14, 2018. Users who are configured for two-way SMS are automatically switched to call to phone verification at that time.|
|Notification through mobile app|Sends a push notification to your phone or registered device. The user views the notification and selects Approve to complete verification. The Microsoft Authenticator app is available for Windows Phone, Android, and iOS. Push notifications through the mobile app provide the best user experience.|
|Verification code from mobile app|The Microsoft Authenticator app generates a new OATH verification code every 30 seconds. The user enters the verification code into the sign-in interface. The Microsoft Authenticator app is available for Windows Phone, Android, and iOS. Verification code from mobile app can be used when the phone has no data connection or cellular signal.|

## Explore multifactor authentication settings

### Account lockout

To prevent repeated MFA attempts as part of an attack, the account lockout settings let you specify how many failed attempts to allow before the account becomes locked out for a period of time. The account lockout settings are only applied when a pin code is entered for the MFA prompt. The following settings are available:

* Number of MFA denials to trigger account lockout
* Minutes until account lockout counter is reset
* Minutes until account is automatically unblocked

### Block and unblock users

If a user's device has been lost or stolen, you can block authentication attempts for the associated account.

### Fraud alerts

* Block user when fraud is reported - Configure the fraud alert feature so that your users can report fraudulent attempts to access their resources. Users can report fraud attempts by using the mobile app or through their phone. Block user when fraud is reported: If a user reports fraud, their account is blocked for 90 days or until an administrator unblocks their account. An administrator can review sign-ins by using the sign-in report and take appropriate action to prevent future fraud. An administrator can then unblock the user's account.
* Code to report fraud during initial greeting - Code to report fraud during initial greeting: When users receive a phone call to perform two-step verification, they normally press # to confirm their sign-in. To report fraud, the user enters a code before pressing #. This code is 0 by default, but you can customize it.

### Notifications

Email notifications can be configured when users report fraud alerts. These notifications are typically sent to identity administrators, as the user's account credentials are likely compromised.

### OATH tokens

Azure AD supports the use of OATH-TOTP SHA-1 tokens that refresh codes every 30 or 60 seconds. Customers can purchase these tokens from the vendor of their choice.

### Trusted IPs

Trusted IPs is a feature to allow federated users or IP address ranges to bypass two-step authentication. Notice there are two selections in this screenshot.

Which selections you can make depends on whether you have managed or federated tenants.

* Managed tenants. For managed tenants, you can specify IP ranges that can skip MFA.
* Federated tenants. For federated tenants, you can specify IP ranges and you can also exempt AD FS claims users.

Important: The Trusted IPs bypass works only from inside of the company intranet. If you select the All Federated Users option and a user signs in from outside the company intranet, the user must authenticate by using two-step verification. The process is the same even if the user presents an AD FS claim.

## Enable multifactor authentication

To enable MFA, go to the User Properties in Azure Active Directory, and then the Multi-Factor Authentication option. From there, you can select the users that you want to modify and enable for MFA. You can also bulk enable groups of users with PowerShell. User's states can be Enabled, Enforced, or Disabled.

Note: on first-time sign-in, after MFA has been enabled, users are prompted to configure their MFA settings. For example, if you enable MFA so that users must use a mobile device, users will be prompted to configure their mobile device for MFA. Users must complete those steps, or they will not be permitted to sign in, which they cannot do until they have validated that their mobile device is MFA-compliant.

All users start out Disabled. When you enroll users in per-user Azure AD Multi-Factor Authentication, their state changes to Enabled. When enabled users sign in and complete the registration process, their state changes to Enforced. Administrators may move users between states, including from Enforced to Enabled or Disabled.

### Enable MFA for Global Admins

Azure MFA is included free of charge for global administrator security. Enabling MFA for global administrators provides an added level of security when managing and creating Azure resources like virtual machines, managing storage, or using other Azure services. Secondary authentication includes phone call, text message, and the authenticator app.

Important: remember you can only enable MFA for organizational accounts stored in Active Directory. These are also called work or school accounts.

## Implement Azure AD conditional access

The old world of security behind a corporate firewall, having your secure network perimeter just doesn’t work anymore, not with people wanting to work from anywhere, being able to connect to all sorts of cloud applications.

Conditional Access is the tool used by Azure Active Directory to bring signals together, to make decisions, and enforce organizational policies. Conditional Access is at the heart of the new **identity driven control plane**.

Conditional access policy is really a next generation policy that’s built for the cloud. It’s able to consider massive amounts of data, as well as contextual data from a user sign-in flow and make sure that the right controls are enforced.

### Identity as a Service - the new control plane

What is the basis for saying that identity management is the new control plane? First, what is the control plane? In a switch or router, the control plane is the part that controls where the traffic is to go, but it’s not responsible for the movement of the traffic. The control plane learns the routes, either static or dynamic. The part responsible for moving the traffic is the forwarding plane. The following figure depicts a simple switch diagram.

A user’s identity is like a control plane, because it controls which protocols the user will interact with, which organizational programs the user can access, and which devices the user can employ to access those programs. Identity is what helps protect user and corporate data. For example, should that data be encrypted, deleted, or ignored when an issue occurs?

Now, everything pivots around that user identity. You know what their activities are, and where they are located. You know what devices they’re using. Then we leverage that information in conditional access policy to be able to enforce things like multifactor authentication or require a compliant device.

There are the conditions, which indicate when the policy is going to apply. This can be, again, the location, type of application that you’re on, any detected risk. How is the risk determined? It is determined from all the analysis and intel that we have across organizations using Azure Active Directory, as well as Microsoft consumer identity offerings. Conditional Access is the tool used by Azure Active Directory to bring signals together, to make decisions, and enforce organizational policies. Conditional Access policies at their simplest are if-then statements, if a user wants to access a resource, then they must complete an action. Example: A payroll manager wants to access the payroll application and is required to perform multifactor authentication to access it.

Administrators are faced with two primary goals:

* Empower users to be productive wherever and whenever
* Protect the organization's assets

By using Conditional Access policies, you can apply the right access controls when needed to keep your organization secure and stay out of your user’s way when not needed.

Signals are verified to determine access to apps.

Conditional Access policies are enforced after the first-factor authentication has been completed. Conditional Access is not intended as an organization's first line of defense for scenarios like denial-of-service (DoS) attacks but can use signals from these events to determine access.

Important: conditional Access is an effective way to enable access to resources after specific conditions have been met.

## Configure conditional access conditions

Conditional access is a capability of Azure AD (with an Azure AD Premium license) that enables you to enforce controls on the access to apps in your environment based on specific conditions from a central location. With Azure AD conditional access, you can factor how a resource is being accessed into an access control decision. By using conditional access policies, you can apply the correct access controls under the required conditions.

Conditional access comes with six conditions: user/group, cloud application, device state, location (IP range), client application, and sign-in risk. You can use combinations of these conditions to get the exact conditional access policy you need.

With access controls, you can either Block Access altogether or Grant Access with more requirements by selecting the desired controls. You can have several options:

* Require MFA from Azure AD or an on-premises MFA (combined with AD FS).
* Grant access to only trusted devices.
* Require a domain-joined device.
* Require mobile devices to use Intune app protection policies.

Requiring more account verification through MFA is a common conditional access scenario. While users may be able to sign in to most of your organization’s cloud apps, you may want more verification for things like your email system, or apps that contain personnel records or sensitive information. In Azure AD, you can accomplish this with a conditional access policy

Important: the Users and Groups condition is mandatory in a conditional access policy. In your policy, you can either select All users or select specific users and groups.

## Implement access reviews

Azure Active Directory (Azure AD) access reviews enable organizations to efficiently manage group memberships, access to enterprise applications, and role assignments. User's access can be reviewed on a regular basis to make sure only the right people have continued access.
Why are access reviews important?

Azure AD enables you to collaborate internally within your organization and with users from external organizations, such as partners. Users can join groups, invite guests, connect to cloud apps, and work remotely from their work or personal devices. The convenience of leveraging the power of self-service has led to a need for better access management capabilities.

* As new employees join, how do you ensure they have the right access to be productive?
* As people move teams or leave the company, how do you ensure their old access is removed, especially when it involves guests?
* Excessive access rights can lead to audit findings and compromises as they indicate a lack of control over access.
* You must proactively engage with resource owners to ensure they regularly review who has access to their resources.

Use access reviews in the following cases

* Too many users in privileged roles: It's a good idea to check how many users have administrative access, how many of them are Global Administrators, and if there are any invited guests or partners that have not been removed after being assigned to do an administrative task. You can recertify the role assignment users in Azure AD roles such as Global Administrators, or Azure resources roles such as User Access Administrator in the Azure AD Privileged Identity Management (PIM) experience.
* When automation is infeasible: You can create rules for dynamic membership on security groups or Microsoft 365 Groups, but what if the HR data is not in Azure AD or if users still need access after leaving the group to train their replacement? You can then create a review on that group to ensure those who still need access should have continued access.
* When a group is used for a new purpose: If you have a group that is going to be synced to Azure AD, or if you plan to enable a sales management application for everyone in the Sales team group, it would be useful to ask the group owner to review the group membership prior to the group being used in a different risk content.
* Business critical data access: for certain resources, it might be required to ask people outside of IT to regularly sign out and give a justification on why they need access for auditing purposes.
* To maintain a policy's exception list: In an ideal world, all users would follow the access policies to secure access to your organization's resources. However, sometimes there are business cases that require you to make exceptions. As the IT admin, you can manage this task, avoid oversight of policy exceptions, and provide auditors with proof that these exceptions are reviewed regularly.
* Ask group owners to confirm they still need guests in their groups: Employee access might be automated with some on premises IAM, but not invited guests. If a group gives guests access to business sensitive content, then it's the group owner's responsibility to confirm the guests still have a legitimate business need for access.
* Have reviews recur periodically: You can set up recurring access reviews of users at set frequencies such as weekly, monthly, quarterly or annually, and the reviewers will be notified at the start of each review. Reviewers can approve or deny access with a friendly interface and with the help of smart recommendations.

Depending on what you want to review, you will create your access review in Azure AD access reviews, Azure AD enterprise apps (in preview), or Azure AD PIM. Using this feature requires an Azure AD Premium P2 license.

Important: Azure AD Premium P2 licenses are **not required** for users with the Global Administrator or User Administrator roles that set up access reviews, configure settings, or apply the decisions from the reviews.

# Configure Azure AD privileged identity management

https://docs.microsoft.com/en-us/learn/modules/azure-ad-privileged-identity-management/

Azure AD Privileged Identity Management (PIM) allows you to manage, control, and monitor access to the most important resources in your organization. You can give just-in-time access and just-enough-access to users to allow them to do their tasks.

## Explore the zero trust model

Gone are the days when security focused on a strong perimeter defense to keep malicious hackers out.

Anything outside the perimeter was treated as hostile, whereas inside the wall, an organization’s systems were trusted. Today's security posture is to assume breach and use the Zero Trust model. Security professionals no longer focus on perimeter defense. Modern organizations have to support access to data and services evenly from both inside and outside the corporate firewall.

This course will serve as your roadmap as you create and move applications and data to Microsoft Azure. Understanding the security services offered by Azure is key in implementing security-enhanced services.

### What does Zero Trust Mean?

The analyst Zero Trust model, states that you should never assume trust but instead continually validate trust. When users, devices, and data all resided inside the organization’s firewall, they were assumed to be trusted. This assumed trust allowed for easy lateral movement after a malicious hacker compromised an endpoint device.

Instead of assuming everything behind the corporate firewall is safe, the Zero Trust model **assumes breach and verifies each request as though it originates from an open network**. Regardless of where the request originates or what resource it accesses, Zero Trust teaches us to never trust, always verify. Every access request is fully authenticated, authorized, and encrypted before granting access. Microsegmentation and least privileged access principles are applied to minimize lateral movement. Rich intelligence and analytics are utilized to detect and respond to anomalies in real time. With most users now accessing applications and data from the internet, most of the components of the transactions—that is, the users, network, and devices—are no longer under organizational control.

The Zero Trust model relies on verifiable user and device trust claims to grant access to organizational resources. No longer is trust assumed based on the location inside an organization's perimeter.

Notice the trust determination components:

* Identity provider. Establishes a user’s identity and related information.
* Device directory. Validates a device and the device integrity.
* Policy evaluation service. Determines whether the user and device conform to security policies.
* Access proxy. Determines which organizational resources can be accessed.

### Implementing a Zero Trust Security model

Migrating to a Zero Trust security model provides for a simultaneously improvement of security over conventional network-based approaches, and to better enable users where they need access. A Zero Trust model requires signals to inform decisions, policies to make access decisions, and enforcement capabilities to implement those decisions effectively.

Signal - to make an informed decision. Zero Trust consider many signal sources - from identity systems to device management and device security tools - to create context-rich insights that help make informed decisions.

Decision - based on organizational policy. The access request and signal are analyzed to deliver a decision based on finely-tuned access policies, delivering granular, organization-centric access control.

Enforcement - of the policy across resources. Decisions are then enforced across the entire digital estate - such as read-only access to SaaS app or remediating compromised passwords with a self-service password reset.

The user is the common denominator of these components. As previously discussed, that is why a user’s identity and how that identity is managed is now called the control plane. If you can’t determine who the user is, you can’t establish a trust relationship for other transactions.

### Guiding principles of Zero Trust

* Verify explicitly. Always authenticate and authorize based on all available data points, including user identity, location, device health, service or workload, data classification, and anomalies.
* Use least privileged access. Limit user access with Just In Time and Just Enough Access (JIT/JEA), risk based adaptive polices, and data protection to protect both data and productivity.
* Assume breach. Minimize blast radius for breaches and prevent lateral movement by segmenting access by network, user, devices, and application awareness. Verify all sessions are encrypted end to end. Use analytics to get visibility, drive threat detection, and improve defenses.

### Microsoft's Zero Trust architecture

Below is a simplified reference architecture for our approach to implementing Zero Trust. The primary components of this process are Intune for device management and device security policy configuration, Azure AD conditional access for device health validation, and Azure AD for user and device inventory.

The system works with Intune, pushing device configuration requirements to the managed devices. The device then generates a statement of health, which is stored in Azure AD. When the device user requests access to a resource, the device health state is verified as part of the authentication exchange with Azure AD.

## Review the evolution of identity management

Microsoft Identity Manager or MIM helps organizations manage the users, credentials, policies, and access within their organizations and hybrid environments. With MIM, organizations can simplify identity lifecycle management with automated workflows, business rules, and easy integration with heterogenous platforms across the datacenter. MIM enables Active Directory Domain Services to have the right users and access rights for on-premises apps. Azure AD Connect can then make those users and permissions available in Azure AD for Microsoft 365 and cloud-hosted apps.

On-premises Active Directory Domain Services, Azure Active Directory (Azure AD), or a hybrid combination of the two all offer services for user and device authentication, identity and role management, and provisioning. Hybrid identities include multiple user identities including Windows Server Active Directory.

Identity has become the common factor among many services, like Microsoft 365 and Xbox Live, where the person is the center of the services. Identity is now the security boundary, the new firewall, the control plane—whichever comparison you prefer. Your digital identity is the combination of who you are and what you’re allowed to do. That is:

Credentials + privileges = digital identity

First step, you need to help protect your privileged accounts.

These identities have more than the normal user rights and, if compromised, allow a malicious hacker to access sensitive corporate assets. Helping secure these privileged identities is a critical step to establishing security assurances for business assets in a modern organization. Cybercriminals target these accounts and other privileged services in their kill chain to carry out their objectives.

### Evolution of identities

Identity management approaches have evolved from traditional, to advanced, to optimal.

Traditional identity approaches

* On-premises identity providers.
* No single sign-on is present between on-premises and cloud apps.
* Visibility into identity risk is very limited.

Advanced identity approaches

* Cloud identity federates with cloud identity systems.
* Conditional access policies gate access and provide remediation actions.
* Analytics improve visibility into identity risk.

Optimal identity approaches

* Passwordless authentication is enabled.
* User, location, devices, and behavior are analyzed in real time.
* Continuous protection to identity risk.

Steps for a passwordless world

* Enforce MFA — Conform to the fast identity online (FIDO) 2.0 standard, so you can require a PIN and a biometric for authentication rather than a password. Windows Hello is one good example, but choose the MFA method that works for your organization.
* Reduce legacy authentication workflows — Place apps that require passwords into a separate user access portal and migrate users to modern authentication flows most of the time. At Microsoft only 10 percent of our users enter a password on a given day.
* Remove passwords — Create consistency across Active Directory Domain Services and Azure Active Directory (Azure AD) to enable administrators to remove passwords from identity directory.

Important: we recommend Azure AD Privileged Identity Management as the service to help protect your privileged accounts.


# Module: Design an enterprise governance strategy

https://docs.microsoft.com/en-us/learn/modules/enterprise-governance/