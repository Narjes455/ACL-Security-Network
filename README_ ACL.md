# Simple VLAN Network with DHCP, ACL, and DNS

## 🛰️ Project Overview

This project demonstrates a segmented local area network (LAN) using VLANs, with DHCP services for dynamic IP assignment, ACLs to enforce access control, and a DNS server to resolve domain names.

It was built and tested using **Cisco Packet Tracer**.

---

## 🧩 Network Design

- **Router:** Central routing between VLANs
- **Switch:** VLAN-enabled
- **Server:** Handles DNS requests
- **PCs:** Distributed across 3 VLANs (Admin, Staff, Guest)

---

## 🔐 VLAN Setup

| VLAN Name | VLAN ID | IP Network       | Devices         |
|-----------|---------|------------------|-----------------|
| Admin     | 10      | 192.168.10.0/24  | PC1             |
| Staff     | 20      | 192.168.20.0/24  | PC2             |
| Guest     | 30      | 192.168.30.0/24  | PC3             |
| Server    | N/A     | 192.168.99.0/24  | DNS Server      |

---

## 📡 DHCP Configuration

Each VLAN receives dynamic IPs from the router using individual DHCP pools with their respective gateway and DNS:

```bash
ip dhcp pool ADMIN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.99.2

ip dhcp pool STAFF
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.99.2

ip dhcp pool GUEST
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.99.2
```

---

## 🚫 ACL Rules

To protect the server:

- **Only Admin and Staff VLANs** can reach the DNS server.
- **Guest VLAN is blocked** from accessing the server.

Example ACL applied to Router interface:

```bash
access-list 100 permit ip 192.168.10.0 0.0.0.255 192.168.99.0 0.0.0.255
access-list 100 permit ip 192.168.20.0 0.0.0.255 192.168.99.0 0.0.0.255
access-list 100 deny ip 192.168.30.0 0.0.0.255 192.168.99.0 0.0.0.255
access-list 100 permit ip any any
```

Applied with:
```bash
interface G0/0
 ip access-group 100 in
```

---

## 🧠 DNS Server Setup

- **IP:** 192.168.99.2
- **Domain:** `server.local`
- Enabled under Services > DNS > `server.local → 192.168.99.2`

---

## ✅ Testing

Use the following to validate:

- `ping server.local` from Admin or Staff PCs → should succeed
- `ping server.local` from Guest PC → should fail (blocked by ACL)
- Verify each PC receives IP via DHCP

---

## 📁 Files Included

- `.pkt` file (Packet Tracer project)
- `README.md` (project documentation)

---

## 🛠️ Author

- 👩‍💻 Created by: **نرجس حسن العمري**
- 🎓 Diploma in Network Systems Administration — *Expected Graduation: Nov 2025*
