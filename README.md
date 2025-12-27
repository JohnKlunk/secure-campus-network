# Secure Campus Network – Cisco Packet Tracer

Dieses Projekt zeigt den Aufbau eines segmentierten und abgesicherten Netzwerks
mit Cisco Packet Tracer. Ziel war es, ein realistisches Campus-Netzwerk mit
mehreren VLANs, zentralem Routing, DHCP, DNS und Zugriffskontrollen (ACLs)
umzusetzen.

Das Projekt dient als Lern- und Portfolio-Projekt im Bereich Networking /
Cybersecurity.

---

## Netzwerkübersicht

Das Netzwerk besteht aus:

- 1 Core-Router (Router-on-a-Stick)
- 1 Core-Switch (S-CORE)
- mehreren Access-Switches
- Endgeräten (Admin, Teacher, Student)
- einem Server-Netz mit Web-, DNS- und DHCP-Server

Die Kommunikation zwischen den VLANs erfolgt ausschließlich über den Router.

---

## VLAN-Struktur

| VLAN | Name     | Zweck                         | IP-Netz            |
|-----:|----------|-------------------------------|--------------------|
| 10   | ADMIN    | Administratoren               | 10.10.10.0 /24     |
| 20   | TEACHER  | Lehrkräfte                    | 10.20.20.0 /24     |
| 30   | STUDENT  | Studierende                   | 10.30.30.0 /24     |
| 40   | SERVER   | Server (Web, DNS, DHCP)       | 10.40.40.0 /24     |

---

## Routing (Router-on-a-Stick)

Der Core-Router routet alle VLANs über Subinterfaces:

- `G0/1.10` → VLAN 10 (Admin)
- `G0/1.20` → VLAN 20 (Teacher)
- `G0/1.30` → VLAN 30 (Student)
- `G0/1.40` → VLAN 40 (Server)

Jedes VLAN besitzt ein eigenes Gateway:
- z. B. VLAN 10 → `10.10.10.1`

---

## Trunking & Switching

- Verbindung zwischen Router und Core-Switch als **802.1Q Trunk**
- Trunks zwischen Core-Switch und Access-Switches
- Endgeräteports als **Access-Ports** im jeweiligen VLAN
- VLAN 1 wird nicht für Endgeräte genutzt

---

## DHCP (zentraler Server)

- DHCP läuft auf einem Server im VLAN 40
- Server-IP: `10.40.40.10`
- IP-Vergabe für VLAN 10, 20 und 30
- Router nutzt **DHCP Relay (`ip helper-address`)**, um DHCP-Anfragen
  aus anderen VLANs an den Server weiterzuleiten

---

## DNS & Webserver

- DNS-Server im VLAN 40
- Webserver im VLAN 40 (`10.40.40.20`)
- Clients können den Webserver per HTTP erreichen

---

## Zugriffskontrolle (ACLs)

### Student VLAN (VLAN 30)
- Kein Zugriff auf Admin- oder Teacher-Netz
- Kein direkter Zugriff auf das Server-Netz
- Zugriff **nur auf den Webserver per HTTP**
- DHCP und DNS erlaubt

### Server-Schutz
- Webserver erlaubt nur HTTP/HTTPS aus definierten VLANs
- ICMP (Ping) zum Server blockiert
- Direkter Zugriff auf Server-Netz eingeschränkt

---

## Tests & Ergebnisse

✔ DHCP funktioniert in allen Client-VLANs  
✔ Inter-VLAN-Routing funktioniert  
✔ Studenten können die Website aufrufen  
✔ Studenten können keine internen Netze pingen  
✔ Admin- und Teacher-Netz sind getrennt  
✔ Server ist vor direktem Zugriff geschützt  

---

## Lernziele des Projekts

- VLAN-Segmentierung verstehen und umsetzen  
- Router-on-a-Stick konfigurieren  
- DHCP Relay korrekt einsetzen  
- Access Control Lists (ACLs) praxisnah anwenden  
- Grundlegende Netzwerksicherheit implementieren  

---

## Verwendete Tools

- Cisco Packet Tracer
- Cisco IOS CLI
- GitHub (Dokumentation & Portfolio)

---

## Hinweis

Dieses Projekt ist ein Lernprojekt und erhebt keinen Anspruch auf vollständige
Enterprise-Security. Es bildet jedoch realistische Grundlagen für Campus- und
Unternehmensnetzwerke ab.

---

## Autor

John Klunk  
Networking & Cybersecurity (Learning Project)
