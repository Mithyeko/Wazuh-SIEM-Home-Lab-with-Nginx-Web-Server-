# Wazuh-SIEM-Home-Lab-with-Nginx-Web-Server-
A fully functional Wazuh SIEM home lab built from scratch, including all the painful real-world troubleshooting: wrong IPs, “Never connected” agents, curl doing nothing, password issues, and more.

-----------------------

📌 Overview

This project is my personal **Security Operations / SIEM home lab**, built to learn how a real SOC environment works.

I deployed a **Wazuh SIEM server** and a **Linux web server (Nginx)**, connected them with the Wazuh agent, and worked through a lot of beginner issues to get to a fully working setup.

This lab shows that I can:

- Install, configure, and manage a **SIEM (Wazuh)**  
- Deploy and connect **agents** on remote servers  
- Collect and inspect **logs** from services like **Nginx** and **SSH**  
- Troubleshoot real-world problems until the environment works end-to-end  

---

## 🧱 Lab Architecture

**Virtualization:** VirtualBox  
**Network:** Home network + VirtualBox networking  

**Machines:**

1. **Wazuh Server**
   - Hostname: `wazuhserver`
   - Role: SIEM / Manager
   - Services: Wazuh manager, Wazuh dashboard (web UI)

2. **Web Server**
   - Hostname: `wazuh-server`
   - Role: Monitored endpoint
   - Services: Nginx web server, Wazuh agent, SSH

The Wazuh server receives logs and events from the web server via the Wazuh agent, then creates alerts and shows them in the Wazuh dashboard.

You can imagine the architecture like this:
```text
                (Your Host PC)
              +----------------+
              |  Web Browser    |
              |  https://wazuhserver:55000 |
              +---------+------+
                        |
                        |  HTTPS (Dashboard)
                        v
              +---------+--------------------+
              |        Wazuh Server         |
              |  - Manager (TCP/1514)       |
              |  - Dashboard (TCP/55000)    |
              +-------------+---------------+
                            ^
                            |  Agent Communication (TCP/1514)
                            |
                 +----------+-----------+
                 |   Web Server (Agent) |
                 | - wazuh-agent        |
                 | - Nginx + SSH logs   |
                 +----------------------+

