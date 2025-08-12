# VLAN Network with DHCP, ACL, DNS, Web, and Mail Servers

## 📋 Overview
هذا المشروع يوضح تصميم وتنفيذ شبكة محلية (LAN) مقسمة باستخدام **VLANs** مع تكوين **DHCP** لتوزيع عناوين IP تلقائياً، و**ACL** للتحكم في الصلاحيات، بالإضافة إلى **DNS** و**Web** و**Mail Servers**.  
تم تنفيذ المشروع واختباره على **Cisco Packet Tracer**.  

---

## 🧩 Network Topology
- **Router (Cisco 2911):** يقوم بعملية Inter-VLAN Routing وتوزيع عناوين IP عبر DHCP.  
- **Switch:** يدعم VLANs مع منافذ Access وTrunk.  
- **Servers:**
  - **DNS Server:** ترجمة أسماء النطاقات إلى عناوين IP.  
  - **Web Server:** استضافة موقع داخلي.  
  - **Mail Server:** إرسال واستقبال البريد الإلكتروني.  
- **PCs:** موزعة على شبكات الإدارة، الموظفين، الضيوف.  

---

## 🔐 VLAN Configuration

| VLAN Name  | VLAN ID | Network            | Devices                                    |
|------------|---------|--------------------|--------------------------------------------|
| Admins     | 10      | 192.168.10.0/24    | PC0, PC1                                   |
| Employees  | 20      | 192.168.20.0/24    | PC2, PC3                                   |
| Guests     | 30      | 192.168.30.0/24    | PC4, PC5                                   |
| Servers    | 100     | 192.168.100.0/24   | DNS Server, Web Server, Mail Server        |

---

## 📡 DHCP Configuration

```bash
ip dhcp pool Admins
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.100.2

ip dhcp pool Employees
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.100.2

ip dhcp pool Guests
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.100.2

ip dhcp pool Servers
 network 192.168.100.0 255.255.255.0
 default-router 192.168.100.1
 dns-server 192.168.100.2
```

---

## 🚫 ACL Rules

تم إعداد Access Control List للسماح لشبكة الضيوف بالوصول فقط إلى الـ Web Server على المنفذ 80 ومنع أي اتصال آخر مع شبكة السيرفرات:  

```bash
access-list 100 permit tcp 192.168.30.0 0.0.0.255 host 192.168.100.10 eq 80
access-list 100 deny ip 192.168.30.0 0.0.0.255 192.168.100.0 0.0.0.255
access-list 100 permit ip any any
interface g0/0.30
ip access-group 100 in
```
---

## 🧠 DNS Server Setup

- *IP:* 192.168.99.2
- *Domain:* server.local
- Enabled under Services > DNS > server.local → 192.168.100.2
---

## ✅ Testing

Use the following to validate:

- ping server.local from Admin or Staff PCs → should succeed
- ping server.local from Guest PC → should fail (blocked by ACL)
- Verify each PC receives IP via DHCP

---

## 📁 Files Included

- .pkt file (Packet Tracer project)
- README.md (project documentation)

---

## 🛠 Author

- 👩‍💻 Created by: *نرجس حسن العمري*
- 🎓 Diploma in Network Systems Administration — Expected Graduation: Jan 2025
