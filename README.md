# Cyber-Deception & SIEM Monitoring Lab

A distributed Security Operations Center (SOC) lab that utilizes deception-based defense to capture, normalize, and alert on unauthorized network activity in real time. This project implements a remote **Cowrie Honeypot** sensor on a Raspberry Pi and streams telemetry to a centralized **Wazuh SIEM** manager.

## 🛠️ Architecture & Data Flow

This lab simulates an enterprise client-server logging environment. The data flows through the following pipeline:

1. **The Attack Layer (Kali Linux):** Initiates unauthorized SSH/Telnet reconnaissance and brute-force simulations targeting Port 22.
2. **The Interception Layer (Raspberry Pi & iptables):** Serves as an edge sensor. Native SSH is moved to a high port, and `iptables` routes malicious traffic transparently into the honeypot sandbox on Port 2222.
3. **The Deception Layer (Cowrie):** Simulates a vulnerable target filesystem, capturing interactive attacker commands (e.g., `whoami`, `netstat`) and committing them to structured JSON logs.
4. **The Shipping Layer (Wazuh Agent):** Constantly monitors the active JSON log stream, handling folder-level permissions to securely transport telemetry to the central network.
5. **The Analysis Layer (Wazuh Manager):** Processes the incoming stream using custom XML decoders and rules, normalizing the unstructured data into searchable, high-priority dashboard alerts.
