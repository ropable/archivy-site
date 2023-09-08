---
date: 08-31-22
id: 61
path: Certifications
tags: []
title: 'SC-900: Security, Compliance, and Identity Fundamentals'
type: bookmark
url: https://docs.microsoft.com/en-us/certifications/security-compliance-and-identity-fundamentals/
---

# SC-900: Security Compliance and Identity Fundamentals
https://learn.microsoft.com/en-us/certifications/security-compliance-and-identity-fundamentals/

# Learning path: Microsoft Security, Compliance, and Identity Fundamentals: Describe the concepts of security, compliance, and identity
https://learn.microsoft.com/en-us/training/paths/describe-concepts-of-security-compliance-identity/

# Describe basic security capabilities in Azure

## Describe Azure DDoS protection

Any company, large or small, can be the target of a serious network attack. The nature of these attacks might be to make a statement, or simply happen because the attacker wanted a challenge.

### Distributed Denial of Service attacks

The aim of a Distributed Denial of Service (DDoS) attack is to overwhelm the resources on your applications and servers, making them unresponsive or slow for genuine users. A DDoS attack will usually target any public-facing device that can be accessed through the internet.

The three most frequent types of DDoS attack are:

* Volumetric attacks: These are volume-based attacks that flood the network with seemingly legitimate traffic, overwhelming the available bandwidth. Legitimate traffic can't get through. These types of attacks are measured in bits per second.
* Protocol attacks: Protocol attacks render a target inaccessible by exhausting server resources with false protocol requests that exploit weaknesses in layer 3 (network) and layer 4 (transport) protocols. These types of attacks are typically measured in packets per second.
* Resource (application) layer attacks: These attacks target web application packets, to disrupt the transmission of data between hosts.

### What is Azure DDoS Protection?

The Azure DDoS Protection service is designed to help protect your applications and servers by analyzing network traffic and discarding anything that looks like a DDoS attack.

Diagram showing network flow into Azure from both customers and attackers, and how Azure DDoS Protection filters out DDoS attacks.

In the diagram above, Azure DDoS Protection identifies an attacker's attempt to overwhelm the network. It blocks traffic from the attacker, ensuring that it doesn't reach Azure resources. Legitimate traffic from customers still flows into Azure without any interruption of service.

Azure DDoS Protection uses the scale and elasticity of Microsoft's global network to bring DDoS mitigation capacity to every Azure region. During a DDoS attack, Azure can scale your computing needs to meet demand. DDoS Protection manages cloud consumption by ensuring that your network load only reflects actual customer usage.

Azure DDoS Protection comes in two tiers:

* Basic: The Basic service tier is automatically enabled for every property in Azure, at no extra cost, as part of the Azure platform. Always-on traffic monitoring and real-time mitigation of common network-level attacks provide the same defenses that Microsoft’s online services use. Azure’s global network is used to distribute and mitigate attack traffic across regions.

* Standard: The Standard service tier provides extra mitigation capabilities that are tuned specifically to Microsoft Azure Virtual Network resources. DDoS Protection Standard is simple to enable and requires no application changes. Protection policies are tuned through dedicated traffic monitoring and machine learning algorithms. Policies are applied to public IP addresses, which are associated with resources deployed in virtual networks, such as Azure Load Balancer and Application Gateway. The DDoS Standard Protection service has a fixed monthly charge that includes protection for 100 resources. Protection for additional resources are charged on a monthly per-resource basis.

Use Azure DDoS to protect your devices and applications by analyzing traffic across your network, and taking appropriate action on suspicious traffic.

## Describe Azure Firewall

Azure Firewall is a managed, cloud-based network security service that protects your Azure virtual network (VNet) resources from attackers. You can deploy Azure Firewall on any virtual network but the best approach is to use it on a centralized virtual network. All your other virtual and on-premises networks will then route through it. The advantage of this model is the ability to centrally exert control of network traffic for all your VNets across different subscriptions.

Diagram showing how Azure Firewall is running on a centralized VNet can protect both cloud-based VNets and your on-premises network.

With Azure Firewall, you can scale up the usage to accommodate changing network traffic flows, so you don't need to budget for peak traffic. Network traffic is subjected to the configured firewall rules when you route it to the firewall as the subnet default gateway.

### Key features of Azure Firewall

Azure Firewall comes with many features, including but not limited to:

* Built-in high availability and availability zones: High availability is built in so there's nothing to configure. Also, Azure Firewall can be configured to span multiple availability zones for increased availability.
* Network and application level filtering: Use IP address, port, and protocol to support fully qualified domain name filtering for outbound HTTP(s) traffic and network filtering controls.
* Outbound SNAT and inbound DNAT to communicate with internet resources: Translate the private IP address of network resources to an Azure public IP address (source network address translation or SNAT) to identify and allow traffic originating from the virtual network to internet destinations. Similarly, inbound internet traffic to the firewall public IP address is translated (Destination Network Address Translation or DNAT) and filtered to the private IP addresses of resources on the virtual network.
* Multiple public IP addresses: These addresses can be associated with Azure Firewall.
* Threat intelligence: Threat intelligence-based filtering can be enabled for your firewall to alert and deny traffic from/to known malicious IP addresses and domains.
* Integration with Azure Monitor: Integrated with Azure Monitor to enable collecting, analyzing, and acting on telemetry from Azure Firewall logs.

Use Azure Firewall to help protect the Azure resources you've connected to Azure Virtual Networks.

## Describe Web Application Firewall

Web applications are increasingly targeted by malicious attacks that exploit commonly known vulnerabilities. Preventing such attacks in application code is challenging. It can require rigorous maintenance, patching, and monitoring.

Web Application Firewall (WAF) provides centralized protection of your web applications from common exploits and vulnerabilities. A centralized WAF helps make security management simpler, improves the response time to a security threat, and allows patching a known vulnerability in one place, instead of securing each individual web application. A WAF also gives application administrators better assurance of protection against threats and intrusions.

## Describe network segmentation in Azure

Segmentation is about dividing something into smaller pieces. An organization, for example, will typically consist of smaller business groups such as human resources, sales, customer service, and more. In an office environment, it's common to see each business group have their own dedicated office space, while members of the same group share an office. This enables members of the same business group to collaborate, while maintaining separation from other groups to address the confidentiality requirements of each business.

The same concept applies with corporate IT networks. The main reasons for network segmentation are:

* The ability to group related assets that are a part of (or support) workload operations.
* Isolation of resources.
* Governance policies set by the organization.

Network segmentation also supports the Zero Trust model and a layered approach to security that is part of a defense in depth strategy.

Assume breach is a principle of the Zero Trust model so the ability to contain an attacker is vital in protecting information systems. When workloads (or parts of a given workload) are placed into separate segments, you can control traffic from/to those segments to secure communication paths. If one segment is compromised, you'll be able to better contain the impact and prevent it from laterally spreading through the rest of your network.

Network segmentation can secure interactions between perimeters. This approach can strengthen an organization's security posture, contain risks in a breach, and stop attackers from gaining access to an entire workload.

### Azure Virtual Network

Azure Virtual Network (VNet) is the fundamental building block for your organization's private network in Azure. VNet is similar to a traditional network that you'd operate in your own data center, but brings with it additional benefits of Azure's infrastructure such as scale, availability, and isolation.

Azure VNet enables organizations to segment their network. Organizations can create multiple VNets per region per subscription, and multiple smaller networks (subnets) can be created within each VNet.

VNets provide network level containment of resources with no traffic allowed across VNets or inbound to the VNet, by default. Communication needs to be explicitly provisioned. This enables more control over how Azure resources in a VNet communicate with other Azure resources, the internet, and on-premises networks.

## Describe Azure Network Security groups

Network security groups (NSGs) let you filter network traffic to and from Azure resources in an Azure virtual network; for example, a virtual machine. An NSG consists of rules that define how the traffic is filtered. You can associate only one network security group to each virtual network subnet and network interface in a virtual machine. The same network security group, however, can be associated to as many different subnets and network interfaces as you choose.

In the highly simplified diagram, shown below, you can see an Azure virtual network with two subnets that are connected to the internet, and each subnet has a virtual machine. Subnet 1 has an NSG assigned to it that's filtering inbound and outbound access to VM1, which needs a higher level of access. In contrast, VM2 could represent a public-facing machine that doesn't require an NSG.

Diagram showing a simplified virtual network with two subnets each with a dedicated virtual machine resource, the first subnet has a network security group and the second subnet doesn't.

### Inbound and outbound security rules

An NSG is made up of inbound and outbound security rules. NSG security rules are evaluated by priority using five information points: source, source port, destination, destination port, and protocol to either allow or deny the traffic. By default, Azure creates a series of rules, three inbound and three outbound rules, to provide a baseline level of security. You can't remove the default rules, but you can override them by creating new rules with higher priorities.

Each rule specifies one or more of the following properties:

* Name: Every NSG rule needs to have a unique name that describes its purpose. For example, AdminAccessOnlyFilter.
* Priority: Rules are processed in priority order, with lower numbers processed before higher numbers. When traffic matches a rule, processing stops. This means that any other rules with a lower priority (higher numbers) won't be processed.
* Source or destination: Specify either individual IP address or an IP address range, service tag (a group of IP address prefixes from a given Azure service), or application security group. Specifying a range, a service tag, or application security group, enables you to create fewer security rules.
* Protocol: What network protocol will the rule check? The protocol can be any of: TCP, UDP, ICMP or Any.
* Direction: Whether the rule should be applied to inbound or outbound traffic.
* Port range: You can specify an individual or range of ports. Specifying ranges enables you to be more efficient when creating security rules.
Action: Finally, you need to decide what will happen when this rule is triggered.

As an example, the table below shows the default inbound rules, which are included in all NSGs. For this example, assume no other inbound rules have been defined for this NSG.

| Name | Priority |	Source | Source ports | Destination | Destination ports | Protocol | Access |
|----|----|----|----|----|----|----|----|
|AllowVNetInBound |65000 |VirtualNetwork |0-65535 |VirtualNetwork |0-65535 |Any |Allow|
|AllowAzureLoadBalancerInBound |65001 |AzureLoadBalancer |0-65535 |0.0.0.0/0 |0-65535 |Any |Allow|
|DenyAllInBound |65500 |0.0.0.0/0 |0-65535| 0.0.0.0/0| 0-65535 |Any |Deny|

* The AllowVNetInBound rule is processed first as it has the lowest priority value. Recall that rules with the lowest priority value get processed first. This rule allows traffic from any Virtual Network (as defined by the VirtualNework service tag) on any port to any Virtual Network on any port, using any protocol. If a match is found for this rule, then no other rules are processed. If no match is found, then the next rule gets processed.

* The AllowAzureLoadBalancerInBound rule is processed second, as its priority value is higher than the AllowVNetInBound rule. This rule allows traffic from any Azure Load Balancer (as defined by the AzureLoadBalancer service tag) on any port to any IP address on any port, using any protocol. If a match is found for this rule, then no other rules are processed. If no match is found, then the next rule gets processed.

* The last rule in this NSG is the DenyAllInBound rule. This rule denies all traffic from any source IP address on any port to any other IP address on any port, using any protocol.

In summary, any virtual network subnet or network interface card to which this NSG is assigned will only allow inbound traffic from an Azure Virtual Network or an Azure load balancer. All other inbound network traffic is denied. Although not shown in this example, there are also three default outbound rules that are included in all NSGs. You can't remove the default rules, but you can override them by creating new rules with higher priorities (lower priority value).

### What is the difference between Network Security Groups (NSGs) and Azure Firewall?

Now that you've learned about both Network Security Groups and Azure Firewall, you may be wondering how they differ, as they both protect Virtual Network resources. The Azure Firewall service complements network security group functionality. Together, they provide better "defense-in-depth" network security. Network security groups provide distributed network layer traffic filtering to limit traffic to resources within virtual networks in each subscription. Azure Firewall is a fully stateful, centralized network firewall as-a-service, which provides network and application-level protection across different subscriptions and virtual networks.

## Describe Azure Bastion and JIT Access

Let’s assume you’ve set up multiple virtual networks that use a combination of NSGs and Azure Firewalls to protect and filter access to the assets and resources, including virtual machines (VMs). You're now protected from external threats, but need to allow your developers and data scientist, who are working remotely, direct access to those VMs.

In a traditional model, you’d need to expose the Remote Desktop Protocol (RDP) and/or Secure Shell (SSH) ports to the internet. These protocols can be used to gain remote access to your VMs. This process creates a significant surface threat that can be exploited by attackers who actively hunt accessible machines with open management ports, like RDP or SSH. When a VM is successfully compromised, it's used as the entry point to attack further resources within your environment.

### Azure Bastion

Azure Bastion is a service you deploy that lets you connect to a virtual machine using your browser and the Azure portal. The Azure Bastion service is a fully platform-managed PaaS service that you provision inside your virtual network. Azure Bastion provides secure and seamless RDP and SSH connectivity to your virtual machines directly from the Azure portal using Transport Layer Security (TLS). When you connect via Azure Bastion, your virtual machines don't need a public IP address, agent, or special client software.

Diagram showing how a user can make a remote desktop connection to an Azure VM using Azure Bastion.

Bastion provides secure RDP and SSH connectivity to all VMs in the virtual network, and peered virtual networks, in which it's provisioned. Using Azure Bastion protects your virtual machines from exposing RDP/SSH ports to the outside world, while still providing secure access using RDP/SSH.

Azure Bastion deployment is per virtual network with support for virtual network peering, not per subscription/account or virtual machine. Once you provision the Azure Bastion service in your virtual network, the RDP/SSH experience is available to all your VMs in the same VNet, as well as peered VNets.
Key features of Azure Bastion

The following features are available:

* RDP and SSH directly in Azure portal: You get to the RDP and SSH session directly in the Azure portal, using a single-click experience.
* Remote session over TLS and firewall traversal for RDP/SSH: From the Azure portal, a connection to the VM, will open an HTML5 based web client that is automatically streamed to your local device. You'll get your Remote Desktop Protocol (RDP) and Secure Shell (SSH) to traverse the corporate firewalls securely. The connection is made secure by using the Transport Layer Security (TLS) protocol to establish encryption.
* No Public IP required on the Azure VM: Azure Bastion opens the RDP/SSH connection to your Azure virtual machine using private IP on your VM. You don't need a public IP.
* No hassle of managing NSGs: A fully managed platform PaaS service from Azure that's hardened internally to provide secure RDP/SSH connectivity. You don't need to apply any NSGs on an Azure Bastion subnet.
* Protection against port scanning: Because you don't need to expose your virtual machines to the internet, your VMs are protected against port scanning by rogue and malicious users located outside your virtual network.
* Hardening in one place to protect against zero-day exploits: Azure Bastion is a fully platform-managed PaaS service. Because it sits at the perimeter of your virtual network, you don’t need to worry about hardening each virtual machine in the virtual network. The Azure platform protects against zero-day exploits by keeping the Azure Bastion hardened and always up to date for you.

Use Azure Bastion to establish secure RDP and SSH connectivity to your virtual machines in Azure.

### Just-in-time access

Just-in-time (JIT) access allows lock down of the inbound traffic to your VMs, reducing exposure to attacks while providing easy access to connect to VMs when needed.

When you enable just-in-time VM access, you can select the ports on the VM to which inbound traffic will be blocked. Microsoft Defender for Cloud, a tool for security posture management and threat protection, ensures "deny all inbound traffic" rules exist for your selected ports in the network security group (NSG) and Azure Firewall rules. These rules restrict access to your Azure VMs’ management ports and defend them from attack.

If other rules already exist for the selected ports, then those existing rules take priority over the new "deny all inbound traffic" rules. If there are no existing rules on the selected ports, then the new rules take top priority in the NSG and Azure Firewall.

When a user requests access to a VM, Defender for Cloud checks that the user has Azure role-based access control (Azure RBAC) permissions for that VM. If the request is approved, Defender for Cloud configures the NSGs and Azure Firewall to allow inbound traffic to the selected ports from the relevant IP address (or range), for the amount of time that was specified. After the time has expired, Defender for Cloud restores the NSGs to their previous states. Connections that are already established are not interrupted.

JIT requires Microsoft Defender for servers to be enabled on the subscription.

## Describe ways Azure encrypts data

Espionage, data theft, and data exfiltration are a real threat to any company. The loss of sensitive data can be crippling and have legal implications. For most organizations, data is their most valuable asset. In a layered security strategy, the use of encryption serves as the last and strongest line of defense.

### Encryption on Azure

Microsoft Azure provides many different ways to secure your data, each depending on the service or usage required.

* Azure Storage Service Encryption helps to protect data at rest by automatically encrypting before persisting it to Azure-managed disks, Azure Blob Storage, Azure Files, or Azure Queue Storage, and decrypts the data before retrieval.
* Azure Disk Encryption helps you encrypt Windows and Linux IaaS virtual machine disks. Azure Disk Encryption uses the industry-standard BitLocker feature of Windows and the dm-crypt feature of Linux to provide volume encryption for the OS and data disks.
* Transparent data encryption (TDE) helps protect Azure SQL Database and Azure Data Warehouse against the threat of malicious activity. It performs real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application.

### What is Azure Key Vault?

Azure Key Vault is a centralized cloud service for storing your application secrets. Key Vault helps you control your applications' secrets by keeping them in a single, central location and by providing secure access, permissions control, and access logging capabilities. It's useful for different kinds of scenarios:

* Secrets management. You can use Key Vault to store securely and tightly control access to tokens, passwords, certificates, Application Programming Interface (API) keys, and other secrets.
* Key management. You can use Key Vault as a key management solution. Key Vault makes it easier to create and control the encryption keys used to encrypt your data.
* Certificate management. Key Vault lets you provision, manage, and deploy your public and private Secure Sockets Layer/ Transport Layer Security (SSL/ TLS) certificates for Azure, and internally connected, resources more easily.
* Store secrets backed by hardware security modules (HSMs). The secrets and keys can be protected either by software or by FIPS 140-2 Level 2 validated HSMs.

Use the various ways in which Azure can encrypt your data to help you secure it whatever the location or state.


# Describe security management capabilities of Azure

## Describe Cloud security posture management

Cloud-based systems are continually evolving and changing as companies move away from on-premises to the cloud. This move makes it difficult for any IT department to know if your data, assets, and resources are as fully protected as they used to be. Even a small misconfiguration of a new feature can increase the attack surface available for cybercriminals to exploit.

Cloud security posture management (CSPM) is a relatively new class of tools designed to improve your cloud security management. It assesses your systems and automatically alerts security staff in your IT department when a vulnerability is found. CSPM uses tools and services in your cloud environment to monitor and prioritize security enhancements and features.

CSPM uses a combination of tools and services:

* Zero Trust-based access control: Considers the active threat level during access control decisions.
* Real-time risk scoring: To provide visibility into top risks.
* Threat and vulnerability management (TVM): Establishes a holistic view of the organization's attack surface and risk and integrates it into operations and engineering decision-making.
* Discover risks: To understand the data exposure of enterprise intellectual property, on sanctioned and unsanctioned cloud services.
* Technical policy: Apply guardrails to audit and enforce the organization's standards and policies to technical systems.
* Threat modeling systems and architectures: Used alongside other specific applications.

The main goal for a cloud security team working on posture management is to continuously report on and improve the organization's security posture by focusing on disrupting a potential attacker's return on investment (ROI).

The function of CSPM in your organization might be spread across multiple teams, or there may be a dedicated team. CSPM can be useful to many teams in your organization:

* Threat intelligence team
* Information technology
* Compliance and risk management teams
* Business leaders and SMEs
* Security architecture and operations
* Audit team

Use CSPM to improve your cloud security management by assessing the environment, and automatically alerting security staff for vulnerabilities.

## Describe Microsoft Defender for Cloud

Microsoft Defender for Cloud is a tool for security posture management and threat protection. It strengthens the security posture of your cloud resources, and with its integrated Microsoft Defender plans, Defender for Cloud protects workloads running in Azure, hybrid, and other cloud platforms.

Microsoft Defender for Cloud fills three vital needs as you manage the security of your resources and workloads in the cloud and on-premises:

* Continuously assess - Know your security posture, identify and track vulnerabilities.
* Secure - Harden all connected resources and services.
* Defend - Detect and resolve threats to resources, workloads, and services.

The features of Microsoft Defender for Cloud, that deliver on these requirements, cover two broad pillars of cloud security: cloud security posture management and cloud workload protection.

### Cloud security posture management (CSPM)

In Microsoft Defender for Cloud, the posture management features provide:

* Visibility - to help you understand your current security situation
* Hardening guidance - to help you efficiently and effectively improve your security

The central feature in Microsoft Defender for Cloud that enables you to achieve those goals is secure score. Microsoft Defender for Cloud continually assesses your resources, subscriptions, and organization for security issues. It then aggregates all the findings into a single score so that you can tell, at a glance, your current security situation: the higher the score, the lower the identified risk level.

Microsoft Defender for Cloud also provides hardening recommendations based on any identified security misconfigurations and weaknesses. Recommendations are grouped into security controls. Each control is a logical group of related security recommendations, and reflects your vulnerable attack surfaces. Your score only improves when you remediate all of the recommendations for a single resource within a control. Use these security recommendations to strengthen the security posture of your organization's Azure, hybrid, and multi-cloud resources.

### Cloud workload protection (CWP)

The second pillar of cloud security is cloud workload protection. Through cloud workload protection capabilities, Microsoft Defender for Cloud is able to detect and resolve threats to resources, workloads, and services. Cloud workload protections are delivered through integrated Microsoft Defender plans, specific to the types of resources in your subscriptions and provide enhanced security features for your workloads. These are described in the next unit.

## Describe the enhanced security of Microsoft Defender for Cloud

Microsoft Defender for Cloud is offered in two modes:

* Microsoft Defender for Cloud (Free) - Microsoft Defender for Cloud is enabled for free on all your Azure subscriptions. Using this free mode provides the secure score and its related features: security policy, continuous security assessment, and actionable security recommendations to help you protect your Azure resources.
* Microsoft Defender for Cloud with enhanced security features - Enabling enhanced security extends the capabilities of the free mode to workloads running in Azure, hybrid, and other cloud platforms, providing unified security management and threat protection across your workloads. Cloud workload protections are delivered through integrated Microsoft Defender plans, specific to the types of resources in your subscriptions and provide enhanced security features for your workloads.

### Defender plans

Microsoft Defender for Cloud includes a range of advanced intelligent protections for your workloads. The workload protections are provided through Microsoft Defender plans specific to the types of resources in your subscriptions. The Microsoft Defender for Cloud plans you can select from are:

* Microsoft Defender for servers adds threat detection and advanced defenses for your Windows and Linux machines.
* Microsoft Defender for App Service identifies attacks targeting applications running over App Service.
* Microsoft Defender for Storage detects potentially harmful activity on your Azure Storage accounts.
* Microsoft Defender for SQL secures your databases and their data wherever they're located.
* Microsoft Defender for Kubernetes provides cloud-native Kubernetes security environment hardening, workload protection, and run-time protection.
* Microsoft Defender for container registries protects all the Azure Resource Manager based registries in your subscription.
* Microsoft Defender for Key Vault is advanced threat protection for Azure Key Vault.
* Microsoft Defender for Resource Manager automatically monitors the resource management operations in your organization.
* Microsoft Defender for DNS provides an additional layer of protection for resources that use Azure DNS's Azure-provided name resolution capability.
* Microsoft Defender for open-source relational protections brings threat protections for open-source relational databases.

These different plans can be enabled separately and will run simultaneously to provide a comprehensive defense for compute, data, and service layers in your environment.

### Enhanced security features

Microsoft Defender plans specific to the types of resources in your subscriptions provide enhanced security features for your workloads. Listed below are some of the enhanced security features.

* Comprehensive endpoint detection and response - Microsoft Defender for servers includes Microsoft Defender for Endpoint for comprehensive endpoint detection and response (EDR).
* Vulnerability scanning for virtual machines, container registries, and SQL resources - Easily deploy a scanner to all of your virtual machines. View, investigate, and remediate the findings directly within Microsoft Defender for Cloud.
* Multi-cloud security - Connect your accounts from Amazon Web Services (AWS) and Google Cloud Platform (GCP) to protect resources and workloads on those platforms with a range of Microsoft Defender for Cloud security features.
* Hybrid security – Get a unified view of security across all of your on-premises and cloud workloads. Apply security policies and continuously assess the security of your hybrid cloud workloads to ensure compliance with security standards. Collect, search, and analyze security data from multiple sources, including firewalls and other partner solutions.
* Threat protection alerts - Monitor networks, machines, and cloud services for incoming attacks and post-breach activity. Streamline investigation with interactive tools and contextual threat intelligence.
* Track compliance with a range of standards - Microsoft Defender for Cloud continuously assesses your hybrid cloud environment to analyze the risk factors according to the controls and best practices in Azure Security Benchmark. When you enable the enhanced security features, you can apply a range of other industry standards, regulatory standards, and benchmarks according to your organization's needs. Add standards and track your compliance with them from the regulatory compliance dashboard.
* Access and application controls - Block malware and other unwanted applications by applying machine learning powered recommendations adapted to your specific workloads to create allowlists and blocklists. Reduce the network attack surface with just-in-time, controlled access to management ports on Azure VMs. Access and application controls drastically reduce exposure to brute force and other network attacks.

Additional benefits include threat protection for the resources connected to the Azure environment and container security features, among others. Some features may be associated with specific defender plans for specific workloads.

## Describe the Azure Security Benchmark and security baselines for Azure

New services and features are released daily in Azure, developers are rapidly publishing new cloud applications built on these services, and attackers are always seeking new ways to exploit misconfigured resources.

The Azure Security Benchmark (ASB) and security baselines for Azure, which are closely related, help organizations secure their cloud solutions on Azure.

### The Azure Security Benchmark

Microsoft has found that using security benchmarks can help organizations quickly secure their cloud deployments and reduce risk to their organization.

The Azure Security Benchmark (ASB) provides prescriptive best practices and recommendations to help improve the security of workloads, data, and services on Azure. The best way to understand the Azure Security Benchmark is to view it on GitHub Azure Security Benchmark V3. Spoiler alert, it's an excel spreadsheet. Some of the key pieces of information in ASB V3 are:

* ASB ID - Each line item in the ASB has an identifier that maps to a specific recommendation.
* Control domain - ASB control domains include network security, data protection, identity management, privileged access, incident response, endpoint security to name just a few. The control domain is best described as high-level feature or activity that isn't specific to a technology or implementation.
* Mapping to industry frameworks - The recommendations included in the ASB map to existing industry frameworks, such as the Center for Internet Security (CIS), the National Institute of Standards and Technology (NIST), and the Payment Card Industry Data Security Standards (PCI DSS) frameworks. This makes security and compliance easier for customer applications running on Azure services.
* Recommendation - For each control domain area there can be many distinct recommendations. Each recommendation captures specific functionality associated with the control domain area and is itself a control. For example, the "Network Security" control domain in ASB v3 has 10 distinct recommendations identified as NS-1 through NS-10. Each of these recommendations describes a specific control under network security.
* Security principle - Each recommendation lists a "Security Principle" that explains the "what" for the control at the technology-agnostic level
* Azure Guidance - Azure Guidance is focused on the "how", elaborating on the relevant technical features and ways to implement the controls in Azure.

Other pieces of information in the ASB include links to information on implementation, links to information about security stakeholders, and guidance on mapping to Azure policy. These aren't shown in the image below. The image below is an excerpt from the Azure Security Benchmark (ASB v3) and is shown as an example of the type of the content that is included in the ASB v3. The image is not intended to show the complete text for any of the line items.

Microsoft Defender for Cloud continuously assesses an organization's hybrid cloud environment to analyze the risk factors according to the controls and best practices in Azure Security Benchmark. Some of the controls used in the ASB include network security, identity and access control, data protection, data recovery, incident response, and more.

### Security baselines for Azure

Security baselines for Azure apply guidance from the Azure Security Benchmark to the specific service for which it's defined. For example, the security baseline for Azure Active Directory applies guidance from the Azure Security Benchmark version 2.0 to Azure Active Directory.

Security baselines for Azure help organizations strengthen their security through improved tooling, tracking, and security features. They also provide organizations a consistent experience when securing their environment. Content in the security baseline is grouped by the control domains defined by the Azure Security Benchmark and that are applicable to the service.

Each Azure security baseline includes the following information:

* Azure ID: The Azure Security Benchmark ID that corresponds to the recommendation.
* Azure control: The content is grouped by control domain area, as listed in the Azure Security Benchmark, and that is applicable to the service for which the security baseline is defined.
* Benchmark Recommendation: This maps to the recommendation for the associated ASB ID (or Azure ID). Each recommendation describes an individual control in a control domain.
* Customer Guidance: The rationale for the recommendation and links to guidance on how to implement it.
* Responsibility: Who is responsible for implementing the control? Possible scenarios are customer responsibility, Microsoft responsibility, or shared responsibility.
* Microsoft Defender for Cloud monitoring: Does Microsoft Defender for Cloud monitor the control?

The image below is an excerpt from the security baseline for Azure AD and is shown as an example of the type of the content that is included in baseline. The image is not intended to show the complete text for any of the line items.

A subset of information from the Azure Active Directory security baseline

Refer to Azure Security Benchmark documentation for a complete listing of the available baselines.


# Describe security capabilities of Microsoft Sentinel


## Define the concepts of SIEM and SOAR

Protecting an organization’s digital estate, resources, assets, and data from security breaches and attacks is an ongoing and escalating challenge. The business world has large numbers of staff working remotely, creating an exploitable window for cybercriminals.

Having a resilient and robust, industry-standard set of tools can help mitigate and prevent these exploits. Security information event management (SIEM) and security orchestration automated response (SOAR) provide security insights and security automation that can enhance an organization's threat visibility and response.

### What is security information and event management (SIEM)?

A SIEM system is a tool that an organization uses to collect data from across the whole estate, including infrastructure, software, and resources. It does analysis, looks for correlations or anomalies, and generates alerts and incidents.

### What is security orchestration automated response (SOAR)?

A SOAR system takes alerts from many sources, such as a SIEM system. The SOAR system then triggers action-driven automated workflows and processes to run security tasks that mitigate the issue.

To provide a comprehensive approach to security, an organization needs to use a solution that embraces or combines both SIEM and SOAR functionality.


## Describe how Microsoft Sentinel provides integrated threat management

Effective management of an organization’s network security perimeter requires the right combination of tools and systems. Microsoft Sentinel is a scalable, cloud-native SIEM/SOAR solution that delivers intelligent security analytics and threat intelligence across the enterprise. It provides a single solution for alert detection, threat visibility, proactive hunting, and threat response.

This diagram shows the end-to-end functionality of Microsoft Sentinel.

* Collect data at cloud scale across all users, devices, applications, and infrastructure, both on-premises and in multiple clouds.
* Detect previously uncovered threats and minimize false positives using analytics and unparalleled threat intelligence.
* Investigate threats with artificial intelligence (AI) and hunt suspicious activities at scale, tapping into decades of cybersecurity work at Microsoft.
* Respond to incidents rapidly with built-in orchestration and automation of common security tasks.

Microsoft Sentinel helps enable end-to-end security operations, in a modern Security Operations Center (SOC). Listed below are some of the key features of Microsoft Sentinel.

### Connect Sentinel to your data

To on-board Microsoft Sentinel, you first need to connect to your security sources. Microsoft Sentinel comes with many connectors for Microsoft solutions, available out of the box and providing real-time integration. Included are Microsoft 365 Defender solutions, and Microsoft 365 sources, including Office 365, Azure AD, and more. In addition, there are built-in connectors to the broader security ecosystem of non-Microsoft solutions. You can also connect your data sources using community-built data connectors listed in the Microsoft Sentinel GitHub repository or by following generic deployment procedures for how to connect your data source to Microsoft Sentinel. Links to information are included in the Learn more section of the summary and resources unit.

### Workbooks

After you connect data sources to Microsoft Sentinel, you can monitor the data using the Microsoft Sentinel integration with Azure Monitor Workbooks. You'll see a canvas for data analysis and the creation of rich visual reports within the Azure portal. Through this integration, Microsoft Sentinel allows you to create custom workbooks across your data. It also comes with built-in workbook templates that allow quick insights across your data as soon as you connect a data source.

### Analytics

Microsoft Sentinel uses analytics to correlate alerts into incidents. Incidents are groups of related alerts that together create an actionable possible-threat that you can investigate and resolve. With analytics in Microsoft Sentinel, you can use the built-in correlation rules as-is, or use them as a starting point to build your own. Microsoft Sentinel also provides machine learning rules to map your network behavior and then look for anomalies across your resources. These analytics connect the dots, by combining low fidelity alerts about different entities into potential high-fidelity security incidents.

### Manage incidents in Microsoft Sentinel

Incident management allows you to manage the lifecycle of the incident. View all related alerts that are aggregated into an incident. You can also triage and investigate. Review all related entities in the incident and additional contextual information meaningful to the triage process. Investigate the alerts and related entities to understand the scope of breach. Trigger playbooks on the alerts grouped in the incident to resolve the threat detected by the alert. You can also do standard incident management tasks like changing status or assigning incidents to individuals for investigation.

### Security automation and orchestration

You can use Microsoft Sentinel to automate some of your security operations and make your security operations center (SOC) more productive. Microsoft Sentinel integrates with Azure Logic Apps, so you can create automated workflows, or playbooks, in response to events. A security playbook is a collection of procedures that can help SOC engineers and analysts of all tiers to automate and simplify tasks and orchestrate a response. Playbooks work best with single, repeatable tasks, and require no coding knowledge.

### Investigation

Currently in preview, Microsoft Sentinel's deep investigation tools help you to understand the scope of a potential security threat and find the root cause. You choose an entity on the interactive graph to ask specific questions, then drill down into that entity and its connections to get to the root cause of the threat.

### Hunting

Use Microsoft Sentinel's powerful hunting search-and-query tools, based on the MITRE framework (a global database of adversary tactics and techniques), to proactively hunt for security threats across your organization’s data sources, before an alert is triggered. After you discover which hunting query provides high-value insights into possible attacks, you can also create custom detection rules based on your query, and surface those insights as alerts to your security incident responders.

While hunting, you can bookmark interesting events. Bookmarking events enables you to return to them later, share them with others, and group them with other correlating events to create a compelling incident for investigation.

### Notebooks

Microsoft Sentinel supports Jupyter notebooks. Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations, and narrative text. You can use Jupyter notebooks in Microsoft Sentinel to extend the scope of what you can do with Microsoft Sentinel data. For example, perform analytics that aren't built in to Microsoft Sentinel, such as some Python machine learning features, create data visualizations that aren't built in to Microsoft Sentinel, such as custom timelines and process trees, or integrate data sources outside of Microsoft Sentinel, such as an on-premises data set.

### Community

The Microsoft Sentinel community is a powerful resource for threat detection and automation. Microsoft security analysts constantly create and add new workbooks, playbooks, hunting queries, and more, posting them to the community for you to use in your environment. You can download sample content from the private community GitHub repository to create custom workbooks, hunting queries, notebooks, and playbooks for Microsoft Sentinel.

## Understand Sentinel costs

Microsoft Sentinel provides intelligent security analytics across your enterprise. The data for this analysis is stored in an Azure Monitor Log Analytics workspace. Billing is based on the volume of data ingested for analysis in Microsoft Sentinel and stored in the Azure Monitor Log Analytics workspace. There are two ways to pay for the Microsoft Sentinel service: Capacity Reservations and Pay-As-You-Go.

* Capacity Reservations: With Capacity Reservations, you're billed a fixed fee based on the selected tier, enabling a predictable total cost for Microsoft Sentinel.
* Pay-As-You-Go: With Pay-As-You-Go pricing, you're billed per gigabyte (GB) for the volume of data ingested for analysis in Microsoft Sentinel and stored in the Azure Monitor Log Analytics workspace.


# Describe threat protection with Microsoft 365 Defender

## Describe Microsoft 365 Defender services

Microsoft 365 Defender is an enterprise defense suite that protects against sophisticated cyberattacks. With Microsoft 365 Defender, you can natively coordinate the detection, prevention, investigation, and response to threats across endpoints, identities, email, and applications.

Microsoft 365 Defender allows admins to assess threat signals from endpoints, applications, email, and identities to determine an attack's scope and impact. It gives greater insight into how the threat occurred, and what systems have been affected. Microsoft 365 Defender can then take automated action to prevent or stop the attack.

Diagram that shows the four aspects that make up the Microsoft 365 Defender suite: identity, endpoints, apps, and email.

Microsoft 365 Defender suite protects:

* Identities with Microsoft Defender for Identity and Azure AD Identity Protection - Microsoft Defender for Identity uses Active Directory signals to identify, detect, and investigate advanced threats, compromised identities, and malicious insider actions directed at your organization.
* Endpoints with Microsoft Defender for Endpoint - Microsoft Defender for Endpoint is a unified endpoint platform for preventative protection, post-breach detection, automated investigation, and response.
* Applications with Microsoft Defender for Cloud Apps - Microsoft Defender for Cloud Apps is a comprehensive cross-SaaS solution that brings deep visibility, strong data controls, and enhanced threat protection to your cloud apps.
* Email and collaboration with Microsoft Defender for Office 365 - Defender for Office 365 safeguards your organization against malicious threats posed by email messages, links (URLs), and collaboration tools.

Use Microsoft Defender to protect your organization against sophisticated cyberattacks. It coordinates your detection, prevention, investigation, and response to threats across endpoints, identities, email, and applications.

## Describe Microsoft Defender for Office 365

Microsoft Defender for Office 365 safeguards your organization against malicious threats posed by email messages, links (URLs), and collaboration tools, including Microsoft Teams, SharePoint Online, OneDrive for Business, and other Office clients.

Microsoft Defender for Office 365 covers these key areas:

* Threat protection policies: Define threat protection policies to set the appropriate level of protection for your organization.
* Reports: View real-time reports to monitor Microsoft Defender for Office 365 performance in your organization.
* Threat investigation and response capabilities: Use leading-edge tools to investigate, understand, simulate, and prevent threats.
* Automated investigation and response capabilities: Save time and effort investigating and mitigating threats.

Microsoft Defender for Office 365 is available in two plans. The plan you choose influences the tools you’ll see and use. It's important to make sure you select the best plan to meet your organization's needs.

### Microsoft Defender for Office 365 Plan 1

This plan offers configuration, protection, and detection tools for your Office 365 suite:

* Safe Attachments: Checks email attachments for malicious content.
* Safe Links: Links are scanned for each click. A safe link remains accessible, but malicious links are blocked.
* Safe Attachments for SharePoint, OneDrive, and Microsoft Teams: Protects your organization when users collaborate and share files by identifying and blocking malicious files in team sites and document libraries.
* Anti-phishing protection: Detects attempts to impersonate your users and internal or custom domains.
* Real-time detections: A real-time report that allows you to identify and analyze recent threats.

### Microsoft Defender for Office 365 Plan 2

This plan includes all the core features of Plan 1, and provides automation, investigation, remediation, and simulation tools to help protect your Office 365 suite:

* Threat Trackers: Provide the latest intelligence on prevailing cybersecurity issues, and allow an organization to take countermeasures before there's an actual threat.
* Threat Explorer: A real-time report that allows you to identify and analyze recent threats.
* Automated investigation and response (AIR): Includes a set of security playbooks that can be launched automatically, such as when an alert is triggered, or manually. A security playbook can start an automated investigation, provide detailed results, and recommend actions that the security team can approve or reject.
* Attack Simulator: Allows you to run realistic attack scenarios in your organization to identify vulnerabilities. These simulations test your security policies and practices, as well as train your employees to increase their awareness and decrease their susceptibility to attacks.
* Proactively hunt for threats with advanced hunting in Microsoft 365 Defender: Advanced hunting is a query-based threat hunting tool that lets you explore up to 30 days of raw data. You can proactively inspect events in your network to locate threat indicators and entities.
* Investigate alerts and incidents in Microsoft 365 Defender: Microsoft Defender for Office 365 P2 customers have access to Microsoft 365 Defender integration to efficiently detect, review, and respond to incidents and alerts.

### Microsoft Defender for Office 365 availability

Microsoft Defender for Office 365 is included in certain subscriptions, such as Microsoft 365 E5, Office 365 E5, Office 365 A5, and Microsoft 365 Business Premium.

If your subscription doesn’t include Defender for Office 365, you can purchase it as an add-on.

Use Microsoft 365 Defender for Office 365 to protect your organization's collaboration tools and messages.

## Describe Microsoft Defender for Endpoint

Microsoft Defender for Endpoint is a platform designed to help enterprise networks protect endpoints. It does so by preventing, detecting, investigating, and responding to advanced threats. Microsoft Defender for Endpoint embeds technology built into Windows 10 and Microsoft cloud services.

This technology includes endpoint behavioral sensors that collect and process signals from the operating system, cloud security analytics that turn signals into insights, detections and recommendations, and threat intelligence to identify attacker tools & techniques, and generate alerts.

Diagram showing the seven aspects of Microsoft Defender Endpoint: Threat and Vulnerability Management, Attack surface reduction, Next-generation protection, Endpoint detection and response, automated investigation and remediation, Microsoft Threat Experts, and Centralized configuration and administration.

Microsoft Defender for Endpoint includes:

* Threat and vulnerability management: A risk-based approach to the discovery, prioritization, and remediation of endpoint vulnerabilities and misconfigurations. It uses sensors on devices to avoid the need for agents or scans, and prioritizes vulnerabilities.
* Attack surface reduction: The attack surface reduction set of capabilities provides the first line of defense in the stack. By ensuring configuration settings are properly set and exploit mitigation techniques are applied, the capabilities resist attacks and exploitation. This set of capabilities also includes network protection and web protection, which regulate access to malicious IP addresses, domains, and URLs; helping prevent apps from accessing dangerous locations
* Next generation protection: Brings together machine learning, big data analysis, in-depth threat resistance research, and the Microsoft cloud infrastructure to protect devices in your enterprise organization.
* Endpoint detection and response: Provides advanced attack detections that are near real time and actionable. Security analysts can prioritize alerts, see the full scope of a breach, and take response actions to remediate threats.
* Automated investigation and remediation: The automated investigation feature uses inspection algorithms and processes used by analysts (such as playbooks) to examine alerts and take quick remediation action to resolve breaches. This process significantly reduces the volume of alerts that must be investigated individually.
* Microsoft Threat Experts: A managed threat hunting service that provides Security Operation Centers (SOCs) with monitoring and analysis tools to ensure critical threats don’t get missed.
* Management and APIs: Provides APIs to integrate with other solutions.

Microsoft Defender for Endpoint includes Microsoft Secure Score for Devices to help you dynamically assess the security state of your enterprise network, identify unprotected systems, and take recommended actions to improve overall security. Microsoft Defender for Endpoint integrates with various components in the Microsoft Defender suite, and with other Microsoft solutions including Intune and Microsoft Defender for Cloud.

Use Microsoft Defender for Endpoint to protect your organization's endpoints and respond to advanced threats.

## Describe Microsoft Defender for Cloud Apps

Moving to the cloud increases flexibility for employees and IT teams. However, it also introduces new challenges and complexities for keeping your organization secure. To get the full benefit of cloud apps and services, an IT team must find the right balance for supporting access while protecting critical data.

Microsoft Defender for Cloud Apps is a Cloud Access Security Broker (CASB). It's a comprehensive cross-SaaS solution that operates as an intermediary between a cloud user and the cloud provider. Microsoft Defender for Cloud Apps provides rich visibility to your cloud services, control over data travel, and sophisticated analytics to identify and combat cyberthreats across all your Microsoft and third-party cloud services. Use this service to gain visibility into Shadow IT by discovering the cloud apps being used. You can control and protect data in the apps after you sanction them to the service.

### What is a Cloud Access Security Broker?

A CASB acts as a gatekeeper to broker real-time access between your enterprise users and the cloud resources they use, wherever they're located, and regardless of the device they're using. CASBs help organizations protect their environment by providing a wide range of capabilities across the following pillars:

* Visibility - Detect cloud services and app use and provide visibility into Shadow IT.
* Threat protection - Monitor user activities for anomalous behaviors, control access to resources through access controls, and mitigate malware.
* Data security - Identify, classify and control sensitive information, protecting against malicious actors.
* Compliance - Assess the compliance of cloud services.

These capability areas represent the basis of the Defender for Cloud Apps framework described below.

### The Defender for Cloud Apps framework

Microsoft Defender for Cloud Apps is built on a framework that provides the following capabilities:

* Discover and control the use of Shadow IT: Identify the cloud apps, and IaaS and PaaS services used by your organization. Investigate usage patterns, assess the risk levels and business readiness of more than 25,000 SaaS apps against more than 80 risks.
* Protect against cyberthreats and anomalies: Detect unusual behavior across cloud apps to identify ransomware, compromised users, or rogue applications, analyze high-risk usage, and remediate automatically to limit risks.
* Protect your sensitive information anywhere in the cloud: Understand, classify, and protect the exposure of sensitive information at rest. Use out-of-the-box policies and automated processes to apply controls in real time across all your cloud apps.
* Assess your cloud apps' compliance: Assess if your cloud apps meet relevant compliance requirements, including regulatory compliance and industry standards. Prevent data leaks to non-compliant apps and limit access to regulated data.

### Microsoft Defender for Cloud Apps functionality

Defender for Cloud Apps Security delivers on the components of the framework through an extensive list of features and functionality. Listed below are some examples.

* Cloud Discovery maps and identifies your cloud environment and the cloud apps your organization uses. Cloud Discovery uses your traffic logs to dynamically discover and analyze the cloud apps being used.
* Sanctioning and unsanctioning apps in your organization by using the Cloud apps catalog that includes over 25,000 cloud apps. The apps are ranked and scored based on industry standards. You can use the cloud app catalog to rate the risk for your cloud apps based on regulatory certifications, industry standards, and best practices.
* Use App connectors to integrate Microsoft and non-Microsoft cloud apps with Microsoft Defender for Cloud Apps, extending control and protection. Defender for Cloud Apps queries the app for activity logs, and it scans data, accounts, and cloud content that can be used to enforce policies, detect threats and provide governance actions to resolve issues.
* Conditional Access App Control protection provides real-time visibility and control over access and activities within your cloud apps. Avoid data leaks by blocking downloads before they happen, setting rules to require data stored in and downloaded from the cloud to be protected with encryption, and controlling access from non-corporate or risky networks.
* Use policies to detect risky behavior, violations, or suspicious data points and activities in your cloud environment. You can use policies to integrate remediation processes to achieve risk mitigation.

### Office 365 Cloud App Security

Office 365 Cloud App Security is a subset of Microsoft Defender for Cloud Apps that provides enhanced visibility and control for Office 365. Office 365 Cloud App Security includes threat detection based on user activity logs, discovery of Shadow IT for apps with similar functionality to Office 365 offerings, control app permissions to Office 365, and apply access and session controls.

It offers a subset of the core Microsoft Defender for Cloud Apps features.

### Enhanced Cloud App Discovery in Azure Active Directory

Azure Active Directory Premium P1 includes Azure Active Directory Cloud App Discovery at no extra cost. This feature is based on the Microsoft Defender for Cloud Apps Cloud Discovery capabilities that provide deeper visibility into cloud app usage in your organization.

It provides a reduced subset of the Microsoft Defender for Cloud Apps discovery capabilities.

Use Microsoft Defender for Cloud Apps to intelligently and proactively identify and respond to threats across your organization's Microsoft and non-Microsoft cloud services.

## Describe Microsoft Defender for Identity

Microsoft Defender for Identity is a cloud-based security solution. It uses your on-premises Active Directory data (called signals) to identify, detect, and investigate advanced threats, compromised identities, and malicious insider actions directed at your organization.

Microsoft Defender for Identity provides security professionals managing hybrid environments functionality to:

* Monitor and profile user behavior and activities.
* Protect user identities and reduce the attack surface.
* Identify and investigate suspicious activities and advanced attacks across the cyberattack kill-chain.
* Provide clear incident information on a simple timeline for fast triage

### Monitor and profile user behavior and activities

Defender for Identity monitors and analyzes user activities and information across your network, including permissions and group membership, creating a behavioral baseline for each user. Defender for Identity then identifies anomalies with adaptive built-in intelligence. It gives insights into suspicious activities and events, revealing the advanced threats, compromised users, and insider threats facing your organization.

### Protect user identities and reduce the attack surface

Defender for Identity provides insights on identity configurations and suggested security best practices. Through security reports and user profile analytics, Defender for Identity helps reduce your organizational attack surface, making it harder to compromise user credentials and advance an attack.

Defender for Identity security reports, help identify users and devices that authenticate using clear-text passwords. It also provides extra insights into how to improve security posture and policies.

For hybrid environments in which Active Directory Federation Services (AD FS) is present, Defender for Identity protects the AD FS by detecting on-premises attacks and providing visibility into authentication events generated by the AD FS.

### Identify suspicious activities and advanced attacks across the cyberattack kill-chain

Typically, attacks are launched against any accessible entity, such as a low-privileged user. Attacks then quickly move laterally until the attacker accesses valuable assets. These assets might include sensitive accounts, domain administrators, and highly sensitive data. Defender for Identity identifies these advanced threats at the source throughout the entire cyberattack kill-chain:

* Reconnaissance
* Compromised credentials
* Lateral movements
* Domain dominance

### Investigate alerts and user activities

Defender for Identity is designed to reduce general alert noise, providing only relevant, important security alerts in a simple, real-time organizational attack timeline.

Use the Defender for Identity attack timeline view and the intelligence of smart analytics to stay focused on what matters. Also, you can use Defender for Identity to quickly investigate threats, and gain insights across the organization for users, devices, and network resources.

Microsoft Defender for Identity protects your organization from compromised identities, advanced threats, and malicious insider actions.

## Describe the Microsoft 365 Defender portal

Microsoft 365 Defender natively coordinates detection, prevention, investigation, and response across endpoints, identities, email, and applications to provide integrated protection against sophisticated attacks. The Microsoft 365 Defender portal brings this functionality together into a central place that is designed to meet the needs of security teams and emphasizes quick access to information, simpler layouts. Through the Microsoft 365 Defender portal you can view the security health of your organization.

The Microsoft 365 Defender portal home page shows many of the common cards that security teams need. The composition of cards and data depends on the user role. Because the Microsoft 365 Defender portal uses role-based access control, different roles will see cards that are more meaningful to their day-to-day jobs.

The cards fall into these categories:

* Identities- Monitor the identities in your organization and keep track of suspicious or risky behaviors.
* Data - Help track user activity that could lead to unauthorized data disclosure.
* Devices - Get up-to-date information on alerts, breach activity, and other threats on your devices.
* Apps - Gain insight into how cloud apps are being used in your organization.

The Microsoft 365 Defender portal allows admins to tailor the navigation pane to meet daily operational needs. Admins can customize the navigation pane to show or hide functions and services based on their specific preferences. Customization is specific to the individual admin, so other admins won’t see these changes.

Note: You must be assigned an appropriate role, such as Global Administrator, Security Administrator, Security Operator, or Security Reader in Azure Active Directory to access the Microsoft 365 Defender portal.

The left navigation pane provides security professionals easy access to the email and collaboration capabilities of Microsoft Defender for Office 365 and the capabilities for Microsoft Defender for Endpoint which were described in the previous units. Listed below we describe a few of the other capabilities accessible from the left navigation bar in the Microsoft 365 Defender portal.

### Incidents and alerts

Microsoft 365 services and apps create alerts when they detect a suspicious or malicious event or activity. Individual alerts provide valuable clues about a completed or ongoing attack. These alerts are automatically aggregated by Microsoft 365 Defender. It's the grouping of these related alerts that form an incident. The incident provides a comprehensive view and context of an attack.

The incidents queue is a central location lists each incident by severity. Selecting an incident name displays a summary of the incident and provides access to tabs with additional information, including:

* All the alerts related to the incident.
* All the users that have been identified to be part of or related to the incident.
* All the mailboxes that have been identified to be part of or related to the incident.
* All the automated investigations triggered by the alerts in the incident.
* All the supported evidence and response.

### Hunting

Advanced hunting is a query-based threat-hunting tool that lets security professionals explore up to 30 days of raw data. Advanced hunting queries enable security professionals to proactively search for threats, malware, and malicious activity across your endpoints, Office 365 mailboxes, and more. Threat-hunting queries can be used to build custom detection rules. These rules run automatically to check for and then respond to suspected breach activity, misconfigured machines, and other findings.
Threat analytics

Threat analytics is our in-product threat intelligence solution from expert Microsoft security researchers. It's designed to assist security teams track and respond to emerging threats. The threat analytics dashboard highlights the reports that are most relevant to your organization. It includes the latest threats, high impact threats (threats with the most active alerts affecting your organization), and high exposure threats.

Selecting a specific threat from the dashboard provides a threat analytics report that provides more detailed information that includes detailed analyst report, impacted assets, mitigations, and much more.

### Secure Score

Microsoft Secure Score, one of the tools in the Microsoft 365 Defender portal, is a representation of a company's security posture. The higher the score, the better your protection. From a centralized dashboard in the Microsoft 365 Defender portal, organizations can monitor and work on the security of their Microsoft 365 identities, apps, and devices.

Secure Score helps organizations:

* Report on the current state of their security posture.
* Improve their security posture by providing discoverability, visibility, guidance, and control.
* Compare benchmarks and establish key performance indicators (KPIs).

Currently Microsoft Secure Score supports recommendations for Microsoft 365 (including Exchange Online), Azure Active Directory, Microsoft Defender for Endpoint, Microsoft Defender for Identity, Microsoft Defender Cloud Apps, and Microsoft Teams. New recommendations are being added to Secure Score all the time.

The image below shows an organization's Secure Score, a breakdown of the score by points, and the improvement actions that can boost the organization's score. Finally, it provides an indication of how well the organization's Secure Score compares to other similar organizations.

There's a secure score for both Microsoft 365 Defender and Microsoft Defender for Cloud, but they're subtly different. Secure score in Microsoft Defender for Cloud is a measure of the security posture of your Azure subscriptions. Secure score in the Microsoft 365 Defender portal is a measure of the security posture of the organization across your apps, devices, and identities.
Learning hub

The Microsoft 365 Defender portal includes a learning hub that bubbles up official guidance from resources such as the Microsoft security blog, the Microsoft security community on YouTube, and the official documentation at docs.microsoft.com.

### Reports

Reports are unified in Microsoft 365 Defender. Admins can start with a general security report, and branch into specific reports about endpoints, email & collaboration. The links here are dynamically generated based upon workload configuration.

### Permissions & roles

Access to Microsoft 365 Defender is configured with Azure Active Directory global roles or by using custom roles.