# Installing Wazuh Agents on Windows & Kali Linux

A practical SIEM lab demonstrating the deployment and enrollment of Wazuh agents on Windows and Kali Linux endpoints using an AWS EC2 Ubuntu Wazuh Manager.

## Lab Environment

| Component | Configuration |
|---|---|
| SIEM Platform | Wazuh |
| Wazuh Manager | Ubuntu Server on AWS EC2 |
| Windows Endpoint | Windows 10 Pro |
| Linux Endpoint | Kali Linux 2026.3 |
| Wazuh Version | 4.14.10 |
| Windows Agent | windows-agent-03 |
| Kali Agent | kali-agent-01 |

## Objectives

- Deploy Wazuh agents on Windows and Kali Linux.
- Configure agent-to-manager communication.
- Understand Wazuh communication and enrollment ports.
- Verify agent enrollment and connectivity.
- Monitor both endpoints through the Wazuh Dashboard.

## Network Ports

| Port | Purpose |
|---|---|
| TCP 1514 | Agent communication |
| TCP 1515 | Agent enrollment |
| TCP 443 | Dashboard HTTPS access |

## Lab Results

Both Windows and Kali Linux agents were successfully enrolled and verified as Active in the Wazuh Dashboard.

## Documentation

The complete step-by-step installation procedure, configuration commands, verification steps, screenshots, and troubleshooting guide will be available in this repository.

## Author

**Faisal CyberX**

Cybersecurity | SIEM | SOC Analyst Level 1 Learning

## References

- [Wazuh Documentation](https://documentation.wazuh.com/current/)
- [Wazuh Agent Installation](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html)
- [Wazuh Agent Enrollment](https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/index.html)

## Lab Screenshots

The following screenshots provide practical evidence of the Windows and Kali Linux Wazuh agent deployment and verification process.

| Screenshot | Evidence |
|---|---|
| [Screenshot 1](screenshots/1.webp) | Lab Step 1 |
| [Screenshot 2](screenshots/2.webp) | Lab Step 2 |
| [Screenshot 3](screenshots/3.webp) | Lab Step 3 |
| [Screenshot 4](screenshots/4.webp) | Lab Step 4 |
| [Screenshot 5](screenshots/5.webp) | Lab Step 5 |
| [Screenshot 6](screenshots/6.webp) | Lab Step 6 |
| [Screenshot 7](screenshots/7.webp) | Lab Step 7 |
| [Screenshot 8](screenshots/8.webp) | Lab Step 8 |
| [Screenshot 9](screenshots/9.webp) | Lab Step 9 |
| [Screenshot 10](screenshots/10.webp) | Lab Step 10 |
| [Screenshot 11](screenshots/11.webp) | Lab Step 11 |
| [Screenshot 12](screenshots/12.webp) | Lab Step 12 |
| [Screenshot 13](screenshots/13.webp) | Lab Step 13 |
| [Screenshot 14](screenshots/14.webp) | Lab Step 14 |
| [Screenshot 15](screenshots/15.webp) | Lab Step 15 |
| [Screenshot 16](screenshots/16.webp) | Lab Step 16 |
