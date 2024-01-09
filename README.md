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

