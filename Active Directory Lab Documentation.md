# 🖥️ Active Directory Lab - Step by Step Guide

A walkthrough of setting up an Active Directory lab using VirtualBox, Windows Server 2019, and Windows 10.

---<img width="1084" height="691" alt="AD-LAB" src="https://github.com/user-attachments/assets/acdfa91e-5928-4805-8882-7beaf69cf42f" />

## 📋 What You Need

- [Oracle VirtualBox](https://www.virtualbox.org/)
- [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
- [Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)
- [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10)

---

## Step 1 - Create the Domain Controller VM

1. Open VirtualBox > click **New**
2. Name it **DC**, set RAM to 2048MB, storage to 20GB+
3. Under **Settings > Network**: Adapter 1 = NAT, Adapter 2 = Internal Network
4. Enable **Shared Clipboard** and **Drag & Drop** (Bidirectional)
5. Boot the VM, attach the Server 2019 ISO, and install **Desktop Experience**

---

## Step 2 - Configure Networking

1. Open **ncpa.cpl** and rename your adapters to **INTERNET** and **INTERNAL**
2. Set a static IP on the INTERNAL adapter:
   - IP: `172.16.0.1`
   - Subnet: `255.255.255.0`
   - DNS: `127.0.0.1`

---

## Step 3 - Install Active Directory

1. Open **Server Manager > Add Roles and Features**
2. Select **Active Directory Domain Services** and install
3. Click **Promote this server to a domain controller**
4. Create a new forest: `mydomain.com`
5. Restart and log in as `MYDOMAIN\Administrator`

---

## Step 4 - Set Up DHCP

1. Add the **DHCP Server** role via Server Manager
2. Create a new scope:
   - Range: `172.16.0.100 - 172.16.0.200`
   - Gateway: `172.16.0.1`
3. Authorize and activate the scope

---

## Step 5 - Set Up NAT

1. Add the **Remote Access** role, select **Routing**
2. Open **Routing and Remote Access**, right-click DC > **Configure & Enable**
3. Select **NAT** and choose the **INTERNET** adapter

---

## Step 6 - Set Up Windows 10 Client

1. Create a new VM in VirtualBox, set network to **Internal Network**
2. Install **Windows 10 Pro**
3. Run `ipconfig` to confirm it got an IP from DHCP
4. Join the domain: **System > Rename this PC (Advanced) > Domain: mydomain.com**
5. Restart and log in with your domain account

---

## 🐛 Troubleshooting

| Problem | Fix |
|---|---|
| No default gateway | Check DHCP scope — gateway should be `172.16.0.1` |
| Can't join domain | Verify DC IP config and firewall settings |
| No internet on client | Make sure NAT and Routing are configured correctly |
| Can't ping 8.8.8.8 | Check Windows Firewall and NAT rules |

---

## 🎓 Credits

Lab based on [Josh Madakor's Active Directory tutorial](https://www.youtube.com/watch?v=MHsI8hJmggI) on YouTube.
