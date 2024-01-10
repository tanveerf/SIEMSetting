# A to Z in SIEM Setting for Cyber Attacks

In this project, I have worked on setting up a framework for Security Information and Event Management using, Microsoft Sentinel and a vulnerable virtual machine in Azure. This setup is designed to log live cyber attacks, transform data to gather GEO location, providing real-time insights into cybersecurity threats. The purpose of this project is to demonstrate the use of a cloud-based SIM and a honeypot for monitoring cyber threats and to instill essential cybersecurity practices.

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

The project has been divided in 5 key sections which will help build the SIEM Model.

#### Section 1: Azure Virtual Machine Setup

I began by creating a virtual machine in Azure to serve as a honeypot. This involved configuring the machine to be exposed to the internet to attract and log cyberattacks.

1. Creating a Virtual Machine: The first step is to create a virtual machine (VM) in Azure. This VM is designed to be exposed to the internet and act as a target for cyber attacks from various countries.
2. Setting Up Resource Group: A new resource group is created for the project, which serves as a logical grouping of Azure resources sharing the same lifespan. The resource group is named, for example, "honeypot lab."
3. Configuring the Virtual Machine: The VM is named (e.g., "honeypot" or "honeypot-vm") and placed in a specific geographic region (e.g., Canada East) that corresponds to the Azure data center's location. Default settings for the image and size of the VM are recommended, with the user creating a username and password for later access.
4. Network Configuration: In the networking step, the configuration includes setting up a network security group, which acts similarly to a firewall. Advanced settings in the network security group are accessed for allowing traffic.

#### Section 2: Configuring a Firewall

The aim of this setup is to make the VM highly discoverable for the lab purposes. This includes ensuring the VM is responsive to various methods of discovery, such as TCP pings, SYN scans, or ICMP pings. The idea is to not block any traffic so that the VM can be quickly found and subsequently attacked.

Under normal circumstances, such open settings are not advisable for VMs due to security risks. However, for the purpose of the lab, the VM is intentionally made enticing and easily discoverable to attract cyber attacks for study and analysis.

1. Firewall Configuration: The firewall is set to be open to the public internet, allowing all traffic to access the VM. This is done by removing the default rule and creating a new inbound rule.
2. Inbound Rule Customization: The newly created inbound rule is configured to allow all types of traffic into the VM. This involves setting the destination port to accept any protocol and giving the rule a low priority (e.g., 100). The rule is named for easy identification (e.g., "danger any in").

#### Section 3: Configuring a Log Analytics Workspace, Sentinel

Integration with Azure Sentinel: Azure Sentinel, serving as a Security Information and Event Management (SIEM) system, is set to connect with this workspace. This integration allows for the display of geographic data of attackers on a map.

1. Creating a Log Analytics Workspace: As the VM is being set up, a Log Analytics Workspace is created to ingest logs from the VM. This includes Windows event logs and custom logs with geographic information to identify the origins of cyber attackers.
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

#### Section 4: Collection of GEO Location Data using API

The Event log in Windows Security has a event ID 4625 that corresponds to 'Failed Login'. Since we have a honeypot setup in the VM these events will represent the 'RDP Brute Force Attack'. Using an API from: https://app.ipgeolocation.io/login we will need to collect additional GEO information from the these events.

1. Remote Desktop Access to VM: The VM is accessed via remote desktop. The public IP address of the VM is used for this purpose, and we need to use the logs in with the credentials created during the VM setup.
2. Security Event Log Review: Inside the VM, the Event Viewer is opened to review security events, focusing on event ID 4625, which indicates failed login attempts. This helps in monitoring unauthorized access attempts.
3. Downloading and Running PowerShell Script: A PowerShell script is downloaded (attached in this repository) and run on the VM. This script is designed to extract IP addresses from failed login attempts and convert them to geographical data using an IP geolocation API.
4. Creation of Custom Log File: The script outputs a custom log file containing geographic information of the attackers. This file is stored in a specific directory (C:\ProgramData) on the VM.
5. Firewall Settings on VM: The VM’s firewall settings are adjusted to respond to ICMP echo requests, making it more discoverable on the internet, which is essential for the honeypot setup.
6. Observation of Attack Data: The script runs continuously, logging each failed login attempt with associated geographic data. This data will later be used to create a geo map in Microsoft Sentinel.

#### Section 5: Transformation of GEO Location Data

1. Creating a Custom Log in Log Analytics Workspace: After minimizing the Azure VM, the next step is to create a custom log in the Log Analytics Workspace to incorporate the custom log with geodata from the VM.
2. Configuring the Custom Log: The custom log is set up in Azure by specifying the file path where the log resides on the VM and providing other necessary details.
4. Extracting Fields from the Log Data: Various fields like latitude, longitude, country, and state are extracted from the raw log data. This is done by going to Tables in Azure's interface and changing the schema to add new columns for latitude, longitude, country and state.
5. Setting up Azure Sentinel: Azure Sentinel is then set up to use the log data. The process involves configuring a new workbook in Sentinel and adding a query to visualize the data on a world map.
6. Extracting Data in New Schema: we will need to run the following KQL to extract lat, long information from the raw data.
```kQL
Failed_RDP_With_GEO_CL
| extend Country_CF = extract("country:([^,]+)", 1, RawData)
| extend Latitude_CF = todouble(extract("latitude:([\\d.]+)", 1, RawData))
| extend Longitude_CF = todouble(extract("longitude:([\\d.]+)", 1, RawData))
```

#### Section 6: Analysis of Results

Finally, the system is left running to collect data on real cyber attacks. The Microsoft Sentinel dashboard is set to auto-refresh every five minutes to display the latest data. Note that, for simplicity of this project, although we have collected country, latitude and longitude data, we also have information regarding the username, state and time of attack. We will the following steps for final analysis. 

1. Configuring Map Visualizations: The map visualization is configured to display data based on latitude and longitude or by country. This setup allows for the geographic plotting of the source of failed login attempts. KQL used to setup the map were:
```KQL
Failed_RDP_With_GEO_CL
| extend Latitude_CF = todouble(extract("latitude:([\\d.]+)", 1, RawData))
| extend Longitude_CF = todouble(extract("longitude:([\\d.]+)", 1, RawData))
| project Latitude_CF, Longitude_CF
| summarize Count = count() by bin(Latitude_CF, 0.1), bin(Longitude_CF, 0.1)
| render worldmap 
```
2. Testing and Verifying the Setup: The setup is tested by intentionally failing to log into the VM, ensuring that the failed attempts are logged, parsed, and displayed correctly on the Sentinel map.
3. Geographic Distribution of Attacks: The VM experienced a significant number of attacks from Singapore, Russia, USA, and Netherlands.
4. Common Attack Patterns: Many attackers attempted to access the VM using common usernames like - administrator, azureuser, student. This underscores the importance of avoiding predictable usernames and implementing strong security measures.

#### Project Outcomes

This project not only reinforces the critical role of SIEM systems in modern cybersecurity but also provides a blueprint for effectively setting up and utilizing such systems to detect, analyze, and respond to cyber threats. Key Achievements:
- Successful Integration of SIEM with a Honeypot: The project adeptly combines Microsoft Sentinel's advanced SIEM capabilities with a honeypot setup in Azure, showcasing a proactive approach to cybersecurity.
- Real-Time Threat Monitoring and Analysis: By logging and analyzing live cyber attacks, the project highlights the importance of continuous monitoring in identifying and understanding potential security breaches.
- Enhanced Threat Intelligence with GEO Data: The inclusion of geographical data adds a valuable dimension to the analysis, allowing for the visualization and better understanding of the origins and distribution of cyber attacks.
- Practical Application of Technologies: The project employs a range of technologies—from PowerShell scripting to Kusto Query Language (KQL) and third-party APIs—demonstrating their practical utility in a real-world cybersecurity setting.
- Valuable Insights for Cybersecurity Best Practices: The project's findings underscore the necessity of robust cybersecurity measures, such as using unpredictable usernames and strong security configurations.
