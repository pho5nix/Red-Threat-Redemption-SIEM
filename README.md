# Red-Threat-Redemption SIEM

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  

## Overview
Red-Threat-Redemption is a comprehensive, open-source SIEM (Security Information and Event Management) stack - name inspired by epic *Red Dead Redemption 2*. Built on Debian 13 in a Proxmox VM, it integrates Elasticsearch & Kibana, Filebeat & Vector for logs parsing, Wazuh Manager for endpoints security, Zeek for network analysis on a secondary SPAN/Port-based NIC, pfSense logs ingestion for Suricata, pfBlockerNG and System logs.  

This repo provides step-by-step guides to deploy a high-performance SIEM for threat detection, log aggregation, and visualization. Optimized for 32GB RAM, 4 cores, and secure configurations—no plaintext passwords, minimal footprint, and best practices for efficiency.

### Key Features
- **Minimal Debian 13 Base**: Hardened OS setup with LVM partitioning for flexible storage.  
- **Elastic Stack with Vector**: Elasticsearch for storage/search, Kibana for visualization, Filebeatand Vector for logs parsing.  
- **Wazuh Integration**: Security monitoring with alerts ingested into Elasticsearch.  
- **Zeek Network Monitoring**: Passive analysis on PCI passthrough NIC.  
- **pfSense Logs**: System syslog, Suricata and pfBlocker logs.  
- **Security Focus**: TLS, keystores for secrets, no swap, high file limits.  

## Prerequisites
- Proxmox VE hypervisor with IOMMU enabled (This guide is focused on Proxmox VE but you can setup the infrastructure in any Hypervisor).  
- Basic Linux knowledge.  
- Hardware: 32GB RAM, 4 cores, 220GB storage, 2 NICs (the secondary NIC should be setup for SPAN/Mirror from your switch, mirroring the port you want to monitor traffic.

## Installation Guides
Follow these guides in sequence. Each is self-contained but builds on the previous.

### 1. Debian 13 Installation and Configuration
[Debian Guide](./docs/01.%20Valentine%20%7C%20Debian%2013.md)
- VM setup in Proxmox.  
- Minimal netinst install with LVM partitioning.  
- Hardening and tuningg.

### 2. ELK Stack & Wazuh Setup
[ELK & Wazuh Guide](./docs/elk-wazuh.md)  
- Install Elasticsearch, Kibana, Filebeat, Vector and Wazuh Manager.  
- Secure configurations (TLS, keystores for passwords).  
- Vector pipeline for Wazuh alerts.  
- Kibana data views.

### 3. Zeek Integration (PCI Passthrough NIC)
[Zeek Guide](./docs/zeek-integration.md)  
- Add and verify PCI NIC.  
- Install and configure Zeek.
- Vector pipeline for Zeek logs.  
- Kibana data view for Zeek.

### 4. pfSense Logs Integration
[pfSense Guide](./docs/pfsense-logs.md) 
- Install and configure Syslog-ng.
- Configure system logs for Syslog-ng.
- Configure Syslog-ng for pfBlocker.
- Configure Syslog-ng for Suricata
- Vector pipelines for ingestion and parsing.  
- Kibana data views for Suricata, pfSense syslog and pfBlocker.

## Usage
1. Clone the repo:  
   ```
   git clone https://github.com/yourusername/red-threat-redemption.git
   ```
2. Follow the guides sequentially.  
3. Access Kibana (use elastic credentials).  
4. Monitor dashboards in Kibana for Wazuh alerts, Zeek events, and pfSense logs.  
5. Tune Vector configs in `/etc/vector/vector.yaml` for additional sources.

## Troubleshooting
Common issues across guides:  
- **Connection Failures**: Verify TLS certs and secrets.  
- **Log Ingestion**: Check vector.yaml, inspect service logs for parse issues.
- **Performance**: Monitor RAM; adjust ES heap if needed.
- Refer to individual guides for tool-specific troubleshooting.

## License
MIT License. See [LICENSE](./LICENSE) for details.

## Acknowledgments
- Inspired by *Red Dead Redemption 2* for thematic naming to separate the componets for a memoprable repo.  
- Thanks to Elastic, Wazuh, Vector, Zeek, and pfSense communities.  
- Built for cybersecurity enthusiasts.

Happy hunting threats!
