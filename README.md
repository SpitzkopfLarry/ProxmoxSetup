# Proxmox Virtual Environment with VLAN-based Network Structure and pfSense Firewall

## 1. Project Objective & Motivation

#### Objective
This project aims to design and implement a virtualized network and server infrastructure using **Proxmox VE** as the hypervisor. The architecture leverages **VLANs** for network segmentation and utilizes **pfSense** as the central firewall to manage and secure network traffic.

This setup aims to simulate a real-world data center environment in a virtualized lab, providing an opportunity to gain hands-on experience in advanced networking and virtualization.

#### Motivation
- Learning and applying professional network techniques (VLAN, routing, firewalling)
- Preparation for network administration & data center environments
- Setting up a scalable, virtual infrastructure for testing and learning purposes

## 2. Hardware & Environment

| **Components**  | **Details**                    |
|-----------------|--------------------------------|
| **Hostsystem**  | Lenovo ThinkCentre M700        |
| **CPU / RAM**   | 4 Cores / 16 GB RAM            |
| **Storage**    | 1 TB SSD                       |
| **Hypervisor**  | Proxmox VE 8.x                 |
| **Network**    | 1x physikalisches Interface `eno1` bridged als `vmbr0` |

## 3. steps towards implementation
### 3.1 Proxmox basic installation
- Proxmox VE ISO downloaded
- Proxmox installed on the host
- Basic configuration (IP address 192.168.1.101, gateway 192.168.1.1)
- Network bridge vmbr0 created, interface eno1 as bridge slave
```shell
nmcli connection add type bridge con-name br0 ifname br0
nmcli connection add type bridge-slave con-name bridge-slave-eno1 ifname eno1 master br0
nmcli connection up br0
```
### 3.2 Bridge & IP-Addresses
#### Problem:
IP conflict (host and Proxmox VM had the same IP 192.168.1.100)
#### Solution:
Host kept `192.168.1.100` <br>
Proxmox-VM got `192.168.1.101`

### 3.3 Configuring Proxmox pveproxy Service 
#### Problem:
WebGUI was not accessible → only listened for IPv6
#### Solution:
Config changed /etc/pve/local/pveproxy.cfg
```shell
listen: 0.0.0.0
```
Reboot services
```shell
systemctl restart pveproxy pvedaemon pvestatd
```

