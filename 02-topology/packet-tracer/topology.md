# 02 – Topology (Packet Tracer)

## Ziel
Diese Topologie bildet ein sicheres Campus-/Uni-Netzwerk in Cisco Packet Tracer ab:
- Segmentierung über VLANs
- Inter-VLAN-Routing per Router-on-a-Stick
- Zentrale Services (DHCP, DNS, Intranet-Webserver)
- Zugriffskontrolle per ACLs (Studenten eingeschränkt)

---

## Physische Struktur (High Level)

**R-CORE (Router 2911)**
- Uplink zum Core Switch über **G0/1**
- Subinterfaces (802.1Q) für VLANs: 10/20/30/40

**S-CORE (Core Switch)**
- Zentraler Switch, verteilt VLANs an Access-Switches über Trunks
- Management-IP im Server-VLAN (SVI VLAN 40): **10.40.40.100**

**Access Switches**
- S-ADMIN (VLAN 10)
- S-TEACHER (VLAN 20)
- S-STUDENT (VLAN 30)
- (Server hängen im VLAN 40 – je nach Aufbau direkt am S-CORE oder separatem Switch)

---

## VLANs & Subnetze (tatsächlich umgesetzt)

| VLAN | Name     | Subnetz         | Gateway (Router) |
|------|----------|------------------|------------------|
| 10   | ADMIN    | 10.10.10.0/24    | 10.10.10.1       |
| 20   | TEACHER  | 10.20.20.0/24    | 10.20.20.1       |
| 30   | STUDENT  | 10.30.30.0/24    | 10.30.30.1       |
| 40   | SERVER   | 10.40.40.0/24    | 10.40.40.1       |

> Hinweis: Es wurden **keine** VLANs 50/60/70 dokumentiert, da sie im finalen Setup nicht genutzt wurden.

---

## Trunking / Port-Design

### Trunks (802.1Q)
- S-CORE ↔ Access Switches: **Trunk**
- S-CORE ↔ R-CORE (Router): **Trunk** (für Router-on-a-stick)

### Access Ports
- Endgeräteports sind **Access** und jeweils dem passenden VLAN zugeordnet:
  - Admin-PCs → VLAN 10
  - Teacher-PCs → VLAN 20
  - Student-PCs → VLAN 30
  - Serverports → VLAN 40

---

## Services (Server VLAN 40)

| Service | Host | IP |
|--------|------|----|
| DHCP Server | DHCP-SRV | 10.40.40.10 |
| DNS Server | DNS-SRV | 10.40.40.11 |
| Intranet Webserver | WEB-SRV | 10.40.40.20 |

### DHCP Relay (ip helper)
DHCP läuft zentral im VLAN 40. Der Router relayed DHCP-Anfragen aus anderen VLANs:
- Auf **G0/1.10**, **G0/1.20**, **G0/1.30** ist `ip helper-address 10.40.40.10` gesetzt.

---

## Security (Kurzüberblick)

### ACL – STUDENT_VLAN30 (Inbound auf G0/1.30)
Ziel: Studenten dürfen:
- DHCP nutzen
- DNS nutzen
- Webserver (HTTP) erreichen
- **nicht** in Admin/Teacher/Server-Netze pingen/zugreifen

Ergebnis aus Tests:
- STUDENT → Zugriff auf **10.40.40.20 (HTTP)** funktioniert
- STUDENT → Pings zu internen Netzen sind blockiert (gewünscht)

---

## Wo liegen die Artefakte?

- Packet Tracer Datei:
  - `02-topology/packet-tracer/secure-campus-network.pkt`
- Screenshots:
  - `02-topology/screenshots/topology.png`

---

## Nächster Schritt
- Screenshots der Topologie exportieren (Packet Tracer → Screenshot) und unter `02-topology/screenshots/` ablegen.
- Danach `03-network-config/` mit den echten Running-Configs befüllen (Router + Switches).
