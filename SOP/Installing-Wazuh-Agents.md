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
