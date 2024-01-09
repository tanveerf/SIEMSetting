# A to Z in SIEM Setting for Cyber Attacks

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

#### Step 4: 
1. Remote Desktop Access to VM: The VM is accessed via remote desktop. The public IP address of the VM is used for this purpose, and the user logs in with the credentials created during the VM setup.
2. Security Event Log Review: Inside the VM, the Event Viewer is opened to review security events, focusing on event ID 4625, which indicates failed login attempts. This helps in monitoring unauthorized access attempts.
3. Downloading and Running PowerShell Script: A PowerShell script is downloaded (or copied) and run on the VM. This script is designed to extract IP addresses from failed login attempts and convert them to geographical data using an IP geolocation API.
4. API Key for Geolocation: The user is instructed to obtain an API key from a geolocation service provider to enable the conversion of IP addresses to geographic data.
5. Creation of Custom Log File: The script outputs a custom log file containing geographic information of the attackers. This file is stored in a specific directory (C:\ProgramData) on the VM.
6. Firewall Settings on VM: The VM’s firewall settings are adjusted to respond to ICMP echo requests, making it more discoverable on the internet, which is essential for the honeypot setup.
7. Observation of Attack Data: The script runs continuously, logging each failed login attempt with associated geographic data. This data will later be used to create a geo map in Azure Sentinel.
8. API Key Limitations: There is a mention of the limitations of the free API key for geolocation service, which allows only a thousand calls per day. An upgrade is suggested for more extensive usage.

#### Step 4: 

1. Creating a Custom Log in Log Analytics Workspace: After minimizing the Azure VM, the next step is to create a custom log in the Log Analytics Workspace to incorporate the custom log with geodata from the VM.
2. Copying Log File from VM: The log file, which is on the VM, is copied to the host machine. This is done by accessing the VM, copying the contents of the log file, and pasting it into a new file on the host machine.
3. Configuring the Custom Log: The custom log is set up in Azure by specifying the file path where the log resides on the VM and providing other necessary details.
4. Extracting Fields from the Log Data: Various fields like latitude, longitude, country, and state are extracted from the raw log data. This is done through Azure's interface, ensuring that the data is correctly parsed.
5. Setting up Azure Sentinel: Azure Sentinel is then set up to use the log data. The process involves configuring a new workbook in Sentinel and adding a query to visualize the data on a world map.
6. Configuring Map Visualizations: The map visualization is configured to display data based on latitude and longitude or by country. This setup allows for the geographic plotting of the source of failed login attempts.
7. Testing and Verifying the Setup: The setup is tested by intentionally failing to log into the VM, ensuring that the failed attempts are logged, parsed, and displayed correctly on the Sentinel map.
8. Monitoring for Cyber Attacks: The system is left running to collect data on real cyber attacks. The Azure Sentinel dashboard is set to auto-refresh every five minutes to display the latest data.

#### Step 5: Analysis of Results
1. Geographic Distribution of Attacks: The VM experienced a significant number of attacks from Russia, Asia, Southeast Asia, and a few from the Netherlands, France, and Chile. Interestingly, fewer attacks than expected were observed from the Washington D.C. and Virginia areas in the United States.
2. Nature of Internet Exposure: The text highlights that any device exposed to the internet, regardless of its significance or the owner's profile, is prone to cyber attacks. This includes both personal and business devices, emphasizing that mere internet presence makes a device a target.
3. Common Attack Patterns: Many attackers attempted to access the VM using common usernames like "administrator." This underscores the importance of avoiding predictable usernames and implementing strong security measures.
4. Importance of Strong Passwords and Multi-Factor Authentication: Given the high number of login attempts from around the world in just 18 hours, the use of strong passwords and multi-factor authentication is stressed as crucial defense mechanisms.
5. Global Reach of Cyber Attacks: The text notes the global nature of cyber attacks, with attempts originating from various continents. However, some regions like Africa and Australia showed fewer attacks in the observed period.
6. Continuous Threat Monitoring: The author expects the number of attacks to increase as the VM becomes more known on the internet, highlighting the need for continuous monitoring of cyber threats.
7. Educational Value and Resumé Building: The author suggests that practicing setting up and monitoring such a VM can be valuable for learning and can be included in a cybersecurity professional's resume.

In summary, the text provides a real-world example of the widespread and indiscriminate nature of cyber attacks, emphasizing the importance of robust cybersecurity practices for any internet-connected device.
