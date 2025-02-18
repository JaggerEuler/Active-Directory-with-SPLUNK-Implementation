# Active Directory configuration with Splunk implementation and generated attack telemetry.

## Introduction and Objective
In this project, I implemented Active Directory (AD) and integrated it with Splunk to monitor and log security events. The process involved setting up multiple virtual machines, configuring Splunk for data collection, and simulating attacks to analyze telemetry in Splunk. This write-up details the steps I followed, challenges encountered, and key takeaways.

### Skills Learned

- Configured and managed an Active Directory domain with users and organizational units
- Set up Splunk to collect and analyze security logs for attack detection
- Conducted brute-force attacks and security tests using Kali Linux and Atomic Red Team
- Configured virtual machines, network settings, and security policies for domain integration.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

Splunk – Used as a Security Information and Event Management (SIEM) system for monitoring security logs.
VirtualBox – Created and managed virtual machines for Active Directory and security testing.
Kali Linux – Performed brute-force attacks to simulate real-world security threats.
Atomic Red Team – Simulated adversary techniques based on the MITRE ATT&CK framework to test detection capabilities.

## Steps
## Environment Setup
### Virtual Machine Configuration
- Installed and configured all necessary virtual machines using VirtualBox.
- Configured a NAT network in VirtualBox to allow communication between machines.
- Assigned a static IP address to the Splunk virtual machine and applied Netplan for network configuration.

### Installing and Configuring Splunk
- Downloaded Splunk for Debian.
- Installed VirtualBox utilities on the Splunk VM.
- Added the Splunk download folder to the VM via VirtualBox and added the Splunk user to the `vboxsf` group.
- I created a shared directory on the VM and mounted the Splunk folder on it.
- Installed Splunk using `dpkg` and configured it to start on boot.
- Logged into Splunk via a web browser using `192.168.10.10:8000` and created an index called `endpoint` for telemetry data.
- Configured Splunk to receive data on port `9997`.

## Active Directory Setup
### Windows 10 Target Machine Configuration
- Configured a static IP address, gateway, and subnet mask.
- Installed the Splunk Universal Forwarder and Sysmon from Microsoft.
- Downloaded an open-source Sysmon configuration from GitHub.
- Created and configured an `inputs.conf` file to define data inputs and forwarding rules.

### Windows Server (Domain Controller) Setup
- Installed Active Directory Domain Services (AD DS) and promoted the server to a domain controller.
- Created a new AD forest, as this was a fresh deployment.
- Configured Organizational Units (OUs) named `IT` and `HR`, each containing a user account.
- Configured DNS settings on the Windows 10 machine to resolve the AD domain.
- Joined the Windows 10 machine to the `jagger.local` domain.

## Security Testing and Attack Simulation
### Brute Force Attack with Crowbar
- Set up a Kali Linux VM with a static IP.
- Installed Crowbar, a brute-force tool, and extracted the `rockyou.txt` wordlist.
- Created a smaller `passwords.txt` file containing only the intended password for demonstration.
- Enabled RDP on the Windows target machine and allowed domain users access.
- Launched a brute-force attack using Crowbar, targeting the `James Smith` account from the `IT` OU.
- Successfully logged in, confirming brute-force vulnerability.

### Analyzing Attack Logs in Splunk
- Filtered logs for `James Smith` within the last 15 minutes.
- Identified `EventCode 4625` (failed logins) occurring multiple times, indicating brute-force attempts.
- Found `EventCode 4624` (successful login), verifying the attack's effectiveness.
- Noted the timestamps of failed logins occurring in quick succession, a key indicator of brute-force attempts.

## Advanced Attack Simulations with Atomic Red Team
- Installed Atomic Red Team on the Windows target machine via PowerShell.
- Created exclusions in Windows Defender to prevent file removals.
- Executed predefined attack scenarios based on the MITRE ATT&CK framework:
  - **T1136.001** (Local Account Creation): Created a local user to simulate persistence.
  - **T1059.001** (PowerShell Execution): Simulated a PowerShell attack.
- Verified alerts in Splunk for newly created local user accounts and PowerShell execution attempts.

## Conclusion
This project demonstrated the implementation of Active Directory in a virtualized environment, the integration of Splunk for real-time security monitoring, and attack simulations for testing security controls. The ability to detect and analyze brute-force attacks and unauthorized user activity using Splunk provides valuable insights for security monitoring and incident response. Future improvements could include additional attack scenarios, fine-tuning Splunk alerts, and integrating more threat intelligence feeds.
