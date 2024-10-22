# Firewall Testing Laboratory

## Overview
This project is part of a firewall testing lab conducted on Linux-based virtual machines (Kali and Ubuntu). The lab explores firewall configurations using **UFW** and **IPTables**, network scanning with **nmap**, and traffic monitoring with **Wireshark**. 

## Objectives
- Configure and test firewall rules on a Kali Linux VM.
- Investigate the behavior of firewalls using network scanning and packet capture tools.
- Analyze firewall responses, including filtered, open, and closed ports.

## Tools Used
- **Kali Linux** (firewall configuration and testing)
- **Ubuntu Linux** (source machine for testing)
- **UFW / IPTables** (firewall tools)
- **nmap** (network scanning)
- **Wireshark** (traffic capture and monitoring)

## Key Commands Used
- Enable UFW: `sudo ufw enable`
- Allow HTTP traffic: `sudo ufw allow http`
- Block IP range: `sudo ufw deny from 192.168.32.0/25 to any port http`
- Network scan: `nmap <Kali IP>`

## Findings
- **Difference between filtered and closed ports**: A filtered port is blocked by the firewall and does not respond to probes, while a closed port responds but has no service listening.
- **Stateful vs Stateless Packet Filtering**: Stateful filtering tracks the state of connections, while stateless filtering examines packets independently.

## Conclusion
This lab demonstrated the importance of properly configured firewalls in securing networks. It highlighted practical differences between stateful and stateless filtering and provided hands-on experience with Linux-based firewalls.

## How to Use
1. Clone this repository: 
   ```bash
   git clone git@github.com:sal0nga/Firewall-Testing-Lab.git



