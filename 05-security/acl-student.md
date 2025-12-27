# ACL – Student VLAN (VLAN 30)

## Ziel
Studierende sollen:
- IP-Adressen per DHCP erhalten
- DNS nutzen können
- Auf den internen Webserver zugreifen können
- **Nicht** auf Admin-, Teacher- oder Server-Netze zugreifen

---

## Netzwerkdaten
- VLAN: 30 (STUDENT)
- Subnetz: 10.30.30.0/24
- Gateway: 10.30.30.1
- DNS-Server: 10.40.40.11
- Webserver: 10.40.40.20

---

## ACL-Konfiguration (Router R-CORE)

```text
ip access-list extended STUDENT_VLAN30
 permit udp any any eq bootps
 permit udp any any eq bootpc
 permit udp any host 10.40.40.11 eq domain
 permit tcp any host 10.40.40.11 eq domain
 permit tcp 10.30.30.0 0.0.0.255 host 10.40.40.20 eq www
 deny ip 10.30.30.0 0.0.0.255 10.10.10.0 0.0.0.255
 deny ip 10.30.30.0 0.0.0.255 10.20.20.0 0.0.0.255
 deny ip 10.30.30.0 0.0.0.255 10.40.40.0 0.0.0.255
 permit ip any any
