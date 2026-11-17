# Installing Wazuh Agents on Windows & Kali Linux

## 1. Project Overview

This project demonstrates the installation, configuration, enrollment, and verification of Wazuh agents on Windows 10 and Kali Linux endpoints connected to an Ubuntu-based Wazuh Manager hosted on AWS EC2.

The purpose of this lab is to understand endpoint monitoring and SIEM agent deployment in a practical environment.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| SIEM Platform | Wazuh |
| Wazuh Manager | Ubuntu Server on AWS EC2 |
| Windows Endpoint | Windows 10 Pro |
| Linux Endpoint | Kali Linux 2026.3 |
| Wazuh Version | 4.14.10 |
| Windows Agent | windows-agent-03 |
| Kali Agent | kali-agent-01 |

## 3. Network Requirements

The following ports are used in this lab:

| Port | Protocol | Purpose |
|---|---|---|
| 1514 | TCP | Agent communication |
| 1515 | TCP | Agent enrollment |
| 443 | TCP | Dashboard HTTPS access |

**AWS Security Group:** Allow the required inbound ports from trusted source IP addresses only. Port 1515 is required for enrollment, while 1514 is used for ongoing agent communication. Port 443 provides Dashboard access.

**Important security notes:**
- Replace `MANAGER_PUBLIC_IP` in the commands with your Wazuh Manager's actual public IP address.
- Do not publish your actual public IP, passwords, authentication keys, or other sensitive information.
- Avoid exposing management services to the entire internet.

---

# Part A — Windows Agent Installation

## 4. Verify Windows Environment

**Where:** Windows PowerShell.

Check the Windows operating system:

```powershell
winver
```
Check network connectivity to the Wazuh Manager:

```powershell
Test-NetConnection MANAGER_PUBLIC_IP -Port 1514
```

A successful connection should show:

```text
TcpTestSucceeded : True
```

If the result is `False`, check the AWS Security Group, manager status, and network configuration before proceeding.

## 5. Download Wazuh Agent

Open PowerShell as Administrator.

Download the Wazuh Agent MSI package:

```powershell
Invoke-WebRequest -Uri "https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.10-1.msi" -OutFile "$env:TEMP\wazuh-agent.msi"
```

## 6. Install and Configure Windows Agent

Run the following command in Administrator PowerShell:

```powershell
msiexec.exe /i "$env:TEMP\wazuh-agent.msi" /q WAZUH_MANAGER="MANAGER_PUBLIC_IP" WAZUH_AGENT_NAME="windows-agent-03"
```

This command installs the Wazuh agent, specifies the manager address, and assigns the agent name.

Start the Wazuh service:

```powershell
Start-Service WazuhSvc
```

Set the service to start automatically:

```powershell
Set-Service -Name WazuhSvc -StartupType Automatic
```

## 7. Verify Windows Agent

Check the service status:

```powershell
Get-Service WazuhSvc
```

Expected status:

```text
Status : Running
```

Check the agent configuration:

```powershell
Select-String -Path "C:\Program Files (x86)\ossec-agent\ossec.conf" -Pattern "<address>"
```

Verify manager connectivity:

```powershell
Test-NetConnection MANAGER_PUBLIC_IP -Port 1514
```

Open the Wazuh Dashboard and navigate to the **Agents** section.

Verify that `windows-agent-03` appears with status **Active**.

**Windows screenshot evidence:**
- Windows operating system information
- Wazuh Agent service running
- Windows agent configuration
- Manager connectivity test
- Windows agent Active in Dashboard

---

# Part B — Kali Linux Agent Installation

## 8. Verify Kali Linux Environment

**Where:** Kali Linux Terminal.

Check the operating system:

```bash
cat /etc/os-release
```

Check connectivity to the Wazuh Manager:

```bash
nc -zv MANAGER_PUBLIC_IP 1514
```

Check enrollment port connectivity:

```bash
nc -zv MANAGER_PUBLIC_IP 1515
```

If `nc` is unavailable, install the required package:

```bash
sudo apt update
sudo apt install netcat-openbsd
```

## 9. Add Wazuh Repository

Import the Wazuh GPG key:

```bash
sudo curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import
```

Set permissions:

```bash
sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```

Add the Wazuh repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
```

Update package information:

```bash
sudo apt update
```

## 10. Install Wazuh Agent

Install the same agent version used in this lab:

```bash
sudo apt install wazuh-agent=4.14.10-1
```

If the package version is unavailable, check the available versions:

```bash
apt-cache policy wazuh-agent
```

Use the version compatible with your Wazuh Manager.

## 11. Configure Kali Linux Agent

Back up the original configuration:

```bash
sudo cp /var/ossec/etc/ossec.conf /var/ossec/etc/ossec.conf.backup
```

Open the configuration file:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Locate the existing `<client>` section.

Configure the manager connection and enrollment inside that section. Update the existing settings instead of creating duplicate sections.

Use the following structure as a reference:

```xml
<client>
  <server>
    <address>MANAGER_PUBLIC_IP</address>
    <port>1514</port>
    <protocol>tcp</protocol>
  </server>
  <enrollment>
    <enabled>yes</enabled>
    <manager_address>MANAGER_PUBLIC_IP</manager_address>
    <port>1515</port>
    <agent_name>kali-agent-01</agent_name>
  </enrollment>
</client>
```

Replace `MANAGER_PUBLIC_IP` with the actual Wazuh Manager public IP.

**Important:**
- Preserve other existing configuration settings.
- Keep the `<enrollment>` section inside `<client>`.
- Do not create duplicate `<client>` or `<enrollment>` sections.
- Ensure the agent name is unique.

Save the file in Nano:

- Press `Ctrl + O`
- Press `Enter`
- Press `Ctrl + X`

## 12. Start and Enable Kali Agent

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable the agent service:

```bash
sudo systemctl enable wazuh-agent
```

Start the agent:

```bash
sudo systemctl start wazuh-agent
```

Check service status:

```bash
sudo systemctl status wazuh-agent --no-pager
```

Expected status:

```text
Active: active (running)
```

## 13. Verify Kali Agent Logs

Check recent agent logs:

```bash
sudo tail -n 30 /var/ossec/logs/ossec.log
```

Look for successful enrollment and manager connection messages.

Verify agent processes:

```bash
sudo /var/ossec/bin/wazuh-control status
```

If the service is running but the agent is not appearing in the Dashboard, investigate the manager address, enrollment, network connectivity, and agent logs.

---

# Part C — Final Verification

## 14. Verify Both Agents in Wazuh Dashboard

Open the Wazuh Dashboard.

Navigate to the **Agents** section.

Verify that both agents appear:

| Agent Name | Operating System | Expected Status |
|---|---|---|
| windows-agent-03 | Windows 10 Pro | Active |
| kali-agent-01 | Kali Linux 2026.3 | Active |

Both endpoints should show **Active**.

Capture a screenshot of the Dashboard displaying both agents.

---

## 15. Troubleshooting

### Agent is not connecting

Check the following:

- Confirm the Wazuh Manager is running.
- Verify the manager IP in the agent configuration.
- Check AWS Security Group inbound rules.
- Confirm TCP port 1514 is reachable.
- Confirm TCP port 1515 is reachable during enrollment.
- Check whether the endpoint has internet/network access.

### Windows Agent service is stopped

Run PowerShell as Administrator:

```powershell
Start-Service WazuhSvc
Get-Service WazuhSvc
```

### Kali Agent service is stopped

Run:

```bash
sudo systemctl restart wazuh-agent
sudo systemctl status wazuh-agent --no-pager
```

### Check Kali Agent logs

```bash
sudo tail -n 50 /var/ossec/logs/ossec.log
```

### Agent is not appearing as Active

Check the following:

1. Manager address and port configuration.
2. AWS Security Group rules.
3. Agent enrollment status.
4. Wazuh agent service status.
5. Agent logs for connection or authentication errors.
6. Whether another agent already uses the same agent name.

Do not manually delete agent registrations or client keys without investigating the issue.

---

## 16. Lab Results

Successfully installed and enrolled Wazuh agents on:

- Windows 10 Pro
- Kali Linux 2026.3

Both endpoints were verified as **Active** in the Wazuh Dashboard connected to an AWS EC2 Ubuntu Wazuh Manager.

### Skills Practiced

- SIEM agent deployment
- Windows endpoint monitoring
- Linux endpoint monitoring
- Agent enrollment and communication
- Network connectivity troubleshooting
- Wazuh Dashboard monitoring
- Basic AWS Security Group configuration

---

## 17. Lab Screenshots

The following screenshots provide practical evidence of the lab activities.

All screenshots are available in the repository's `screenshots` directory.

| Screenshot | Link |
|---|---|
| Screenshot 1 | [View](../screenshots/1.webp) |
| Screenshot 2 | [View](../screenshots/2.webp) |
| Screenshot 3 | [View](../screenshots/3.webp) |
| Screenshot 4 | [View](../screenshots/4.webp) |
| Screenshot 5 | [View](../screenshots/5.webp) |
| Screenshot 6 | [View](../screenshots/6.webp) |
| Screenshot 7 | [View](../screenshots/7.webp) |
| Screenshot 8 | [View](../screenshots/8.webp) |
| Screenshot 9 | [View](../screenshots/9.webp) |
| Screenshot 10 | [View](../screenshots/10.webp) |
| Screenshot 11 | [View](../screenshots/11.webp) |
| Screenshot 12 | [View](../screenshots/12.webp) |
| Screenshot 13 | [View](../screenshots/13.webp) |
| Screenshot 14 | [View](../screenshots/14.webp) |
| Screenshot 15 | [View](../screenshots/15.webp) |
| Screenshot 16 | [View](../screenshots/16.webp) |

---

## 18. References

- [Official Wazuh Documentation](https://documentation.wazuh.com/current/)
- [Wazuh Agent Installation Guide](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html)
- [Wazuh Agent Enrollment](https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/index.html)
- [Wazuh Windows Agent Deployment](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)
- [Wazuh Agent Enrollment Troubleshooting](https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/troubleshooting.html)

---

## Author

**Faisal CyberX**

Cybersecurity | SIEM | SOC Analyst Level 1 Learning

GitHub: https://github.com/faisalcyberx
