# Proxmox Virtual Environment with VLAN-based Network Structure and pfSense Firewall

## 1. Project Objective & Motivation

#### Objective
This project aims to design and implement a virtualized network and server infrastructure using **Proxmox VE** as the hypervisor. The architecture leverages **VLANs** for network segmentation and utilizes **pfSense** as the central firewall to manage and secure network traffic.

The setup simulates a real-world data center environment in a virtualized lab, providing an opportunity to gain hands-on experience in advanced networking and virtualization.

#### Motivation
- Learn and apply professional network techniques (VLAN, routing, firewalling)
- Prepare for network administration & data center environments
- Set up a scalable, virtual infrastructure for testing and learning purposes

---

## 2. Hardware & Environment

| **Component**   | **Details**                    |
|-----------------|--------------------------------|
| **Host System** | Lenovo ThinkCentre M700        |
| **OS**          | Ubuntu 24.04.2 LTS             |
| **Kernel**      | Linux 6.11.0-19-generic        |
| **CPU / RAM**   | 4 Cores / 16 GB RAM            |
| **Storage**     | 1 TB SSD                       |
| **Hypervisor**  | Proxmox VE 8.x                 |
---

## 3. Steps Towards Implementation

### 3.1 Proxmox Basic Installation
- Downloaded the latest **Proxmox VE ISO**
- Installed Proxmox on the host system
- Performed basic configuration:
  - IP Address: `192.168.1.101`
  - Gateway: `192.168.1.1`
- Created a network bridge `vmbr0`, with interface `eno1` as the bridge slave

```shell
nmcli connection add type bridge con-name br0 ifname br0
nmcli connection add type bridge-slave con-name bridge-slave-eno1 ifname eno1 master br0
nmcli connection up br0
```

---

### 3.2 Bridge & IP Addresses
#### Problem
There was an IP conflict:  
The **host** and the **Proxmox VM** had the same IP address `192.168.1.100`.

#### Solution
- The **host** kept `192.168.1.100`  
- The **Proxmox VM** was assigned `192.168.1.101`

---

### 3.3 Configuring Proxmox pveproxy Service
#### Problem
The WebGUI was not accessible – it was only listening on IPv6.

#### Solution
Modified the `pveproxy` configuration in `/etc/pve/local/pveproxy.cfg`:

```shell
listen: 0.0.0.0
```

Restarted the relevant services to apply changes:

```shell
systemctl restart pveproxy pvedaemon pvestatd
```
