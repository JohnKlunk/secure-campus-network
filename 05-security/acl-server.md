# ACL – Server/Webserver Schutz (VLAN 40)

## Ziel
Der interne Webserver (10.40.40.20) soll:
- aus **Admin (VLAN 10)** und **Teacher (VLAN 20)** per **HTTP/HTTPS** erreichbar sein
- optional auch aus **Student (VLAN 30)** per **HTTP** erreichbar sein (falls gewünscht)
- **nicht per Ping/ICMP** erreichbar sein (optional, „stealth“)
- **keine anderen Zugriffe** (z. B. SSH, RDP, SMB, etc.) von Clients zulassen

> Hinweis: Diese ACL schützt **nur den Webserver** (10.40.40.20), nicht automatisch das gesamte Server-VLAN.

---

## Netzwerkdaten
- VLAN: 40 (SERVER)
- Subnetz: 10.40.40.0/24
- Gateway: 10.40.40.1
- Webserver: 10.40.40.20
- Admin-Netz: 10.10.10.0/24
- Teacher-Netz: 10.20.20.0/24
- Student-Netz: 10.30.30.0/24

---

## ACL-Konfiguration (Router R-CORE)

### Admin + Teacher dürfen HTTP/HTTPS, Students nur HTTP
```bash
ip access-list extended SERVER_PROTECT
 remark --- Allow Web from Admin + Teacher ---
 permit tcp 10.10.10.0 0.0.0.255 host 10.40.40.20 eq www
 permit tcp 10.10.10.0 0.0.0.255 host 10.40.40.20 eq 443
 permit tcp 10.20.20.0 0.0.0.255 host 10.40.40.20 eq www
 permit tcp 10.20.20.0 0.0.0.255 host 10.40.40.20 eq 443

 remark --- Allow Web from Students (HTTP only) ---
 permit tcp 10.30.30.0 0.0.0.255 host 10.40.40.20 eq www

 remark --- Optional: block ICMP (Ping) to the Webserver ---
 deny icmp any host 10.40.40.20

 remark --- Block everything else to the Webserver ---
 deny ip any host 10.40.40.20

 remark --- Allow all other traffic (so other servers in VLAN40 stay reachable if needed) ---
 permit ip any any
