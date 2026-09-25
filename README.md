# Wazuh SIEM — Air-Gapped Deployment

## About the Project
This project covers the deployment of a centralized **Wazuh SIEM system** in a closed (**air-gapped**) environment running on **Ubuntu Server 26.04 LTS**.

## Architecture
* **Server-Security** (Ubuntu 26.04): Wazuh Indexer + Manager + Dashboard (Docker)
* **Server-Prod / mainserv** (Ubuntu): PostgreSQL + Wazuh Agent
* **Kali Linux**: Security and penetration testing

**Network:** VirtualBox configured with Bridged + Host-only adapters (isolated network segment).

## Tech Stack
* **Wazuh 4.14.7** (Indexer, Manager, Dashboard)
* **Docker** & **Docker Compose**
* **OpenSearch**, **Filebeat**
* **Ubuntu Server 26.04 LTS**, **Kali Linux**
* **LVM**, **Bash**, **SSL/TLS**

## Key Implementations
* Deployed **Wazuh SIEM** (all three components) using **Docker**.
* Prepared the host machine: expanded **LVM from 28 GB to 50 GB** and configured `vm.max_map_count=262144`.
* Generated custom **SSL/TLS certificates** for secure cluster communication.
* Added **4 volume mounts** to prevent uncontrolled writable layer growth in Docker containers.
* Disabled the **Vulnerability Detector** module (as it requires an active internet connection to update databases).
* Enrolled the **Wazuh Agent** on the PostgreSQL production server.
* Configured advanced log monitoring for **PostgreSQL**, `auth.log`, and `syslog`.
* Validated detection capabilities against **SSH brute-force** and **failed sudo** attempts, with full **MITRE ATT&CK** mapping.

## Troubleshooting & Solutions

| Issue | Solution |
| :--- | :--- |
| Broken `dpkg` manager on Ubuntu 26.04 | Switched from native package installation to Docker |
| `No space left on device` error | Expanded LVM storage pool up to 50 GB |
| `ClusterManagerNotDiscoveredException` | Caused by a completely full disk; resolved via storage cleanup |
| Manager writable layer grew to 8.63 GB | Set up 4 dedicated persistent volume mounts |
| `wazuh_queue` size swelled to 10.82 GB | Disabled the offline-incompatible Vulnerability Detector |

## Author
[Sam-Dev.Sec] — https://github.com/Sam-DevSec/wazuh-siem-homelab
