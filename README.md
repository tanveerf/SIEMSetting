## A to Z in SIEM Setting for Cyber Attacks

In this project, I have worked on setting up Microsoft Sentinel and a vulnerable virtual machine in Azure. This setup is designed to log live cyber attacks, providing real-time insights into cybersecurity threats. The purpose of this project is to demonstrate the use of a cloud-based SIM and a honeypot for monitoring cyber threats and to impart essential cybersecurity practices.

### Technologies Used
- Microsoft Sentinel (Security Information and Event Management - SIEM)
- Azure Log Analytics Workspace
- Virtual Machines (VM) in Azure
- PowerShell
- KQL(Kusto Query Language)
- Third-party Geolocation API

### Project Seteup and Installations

- Create Azure Virtual Machine: Set up a virtual machine in Azure to act as a honeypot.
- Configure Log Analytics Workspace: Ingest logs from the virtual machine into Azure's Log Analytics Workspace. Custom logs with GEO data (lattitude, longitude, state/province, and country) using third party API.
- PowerShell Scripting: Utilize PowerShell to extract and analyze attack data.
- Microsoft Sentinel Integration: Connect the virtual machine with Sentinel for real-time attack visualization.

### Project Architecture


### Project Steps

#### Step 1: Azure Virtual Machine Setup

I began by creating a virtual machine in Azure to serve as a honeypot. This involved configuring the machine to be exposed to the internet to attract and log cyberattacks.

1. Creating a Virtual Machine: The first step is to create a virtual machine (VM) in Azure. This VM is designed to be exposed to the internet and act as a target for cyber attacks from various countries.
2. Setting Up Resource Group: A new resource group is created for the project, which serves as a logical grouping of Azure resources sharing the same lifespan. The resource group is named, for example, "honeypot lab."
3. Configuring the Virtual Machine: The VM is named (e.g., "honeypot" or "honeypot-vm") and placed in a specific geographic region (e.g., West US2) that corresponds to the Azure data center's location. Default settings for the image and size of the VM are recommended, with the user creating a username and password for later access.
4. Network Configuration: In the networking step, the configuration includes setting up a network security group, which acts similarly to a firewall. Advanced settings in the network security group are accessed for allowing traffic.

#### Step 2: Configuring a Firewall

1. Firewall Configuration: The firewall is set to be open to the public internet, allowing all traffic to access the VM. This is done by removing the default rule and creating a new inbound rule.
2. Inbound Rule Customization: The newly created inbound rule is configured to allow all types of traffic into the VM. This involves setting the destination port to accept any protocol and giving the rule a low priority (e.g., 100). The rule is named for easy identification (e.g., "danger any in").
3. Purpose of the Configuration: The aim of this setup is to make the VM highly discoverable for the lab purposes. This includes ensuring the VM is responsive to various methods of discovery, such as TCP pings, SYN scans, or ICMP pings. The idea is to not block any traffic so that the VM can be quickly found and subsequently attacked.
4. Contextual Note: It is emphasized that under normal circumstances, such open settings are not advisable for VMs due to security risks. However, for the purpose of the lab, the VM is intentionally made enticing and easily discoverable to attract cyber attacks for study and analysis.

#### Step 3: Configuring a Log Analytics Workspace, Sentinel

1. Creating a Log Analytics Workspace: As the VM is being set up, a Log Analytics Workspace is created to ingest logs from the VM. This includes Windows event logs and custom logs with geographic information to identify the origins of cyber attackers.
2. Integration with Azure Sentinel: Azure Sentinel, serving as a Security Information and Event Management (SIEM) system, is set to connect with this workspace. This integration allows for the display of geographic data of attackers on a map.
3. Configuring the Workspace and VM Connection:
   - The workspace is created within the previously established "honeypot lab" resource group.
   - It's named (e.g., "la honeypot one") and assigned to the West US 2 region.
   - After creating the workspace, the focus shifts to Azure's Security Center to enable log gathering from the VM into the workspace.
4. Security Center Settings:
   - Access the Security Center, then go to "Pricing and Settings" and select the newly created workspace.
   - Turn on Defender and disable SQL servers (as they are not needed for this project).
   - Under "Data Collection," set it to collect all events and save this configuration.
5. Linking VM with Log Analytics Workspace:
   - Return to the Log Analytics Workspace and connect it to the VM by selecting the VM and clicking "connect."
6. Setting up Azure Sentinel:
   - Open a new tab in the browser and navigate to the Azure portal.
   - Begin setting up Azure Sentinel, which is described as a quick process.

   
