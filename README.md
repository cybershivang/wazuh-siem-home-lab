# Wazuh SIEM Home Lab (Kali Linux + Windows Agent)

Hands-on SOC lab: a Wazuh 4.9.2 SIEM server on Kali Linux (VMware, bridged network) monitoring a Windows 10 endpoint.

## Architecture
- **Wazuh server + dashboard:** Kali Linux VM (VMware Workstation, Bridged), v4.9.2
- **Endpoint:** Windows 10 host running Wazuh agent 4.9.2 (agent name: `win-pc`)
- **Agent to server:** TCP 1514 (events), encrypted with AES

## What is working (verified)
- Wazuh server and dashboard running on Kali
- Windows agent registered and **Active** (confirmed via `agent_control -l` and the dashboard Agents Summary)
- Security Configuration Assessment (CIS Windows 10 Enterprise policy) scan completed on the endpoint
- Real-time File Integrity Monitoring (syscheck) started on a custom folder (`Downloads\wazuh-test`)

## In progress
- Verifying FIM alerts (file create / modify / delete) in the dashboard
- Brute-force detection test (failed Windows logons) mapped to MITRE ATT&CK T1110
- FIM events mapped to T1565 (Data Manipulation) and T1070 (Indicator Removal)

## Troubleshooting log (what I learned)
| Problem | Cause | Fix |
|---|---|---|
| Agent not visible, ping failing | I used the VPN `tun0` IP instead of the real LAN IP (`eth0`) | Used the `eth0` address as manager IP |
| `WazuhSvc` not found, no `ossec.conf` | Broken / incomplete agent install | Clean reinstall via MSI with `WAZUH_MANAGER` set |
| Version mismatch risk | Newer agent MSI vs 4.9.2 server | Matched agent version to the server (agent must not be newer than manager) |
| Config edits denied | Editing `Program Files` without admin rights | Ran Notepad / PowerShell as Administrator |

## Skills demonstrated
SIEM deployment, log collection, agent management, endpoint configuration assessment, file integrity monitoring, network troubleshooting, Linux and Windows administration, MITRE ATT&CK mapping.

## Screenshots
_Add: Agents Summary (Active 1), `agent_control -l` output, SCA results, FIM events (once working)._

## Author
Shivang, aspiring SOC Analyst
