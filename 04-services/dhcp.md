# DHCP Service (Central Server + Relay)

## Ziel
Alle Clients in den VLANs **10 (Admin)**, **20 (Teacher)** und **30 (Student)** erhalten ihre IPs automatisch über einen zentralen DHCP-Server im **VLAN 40 (Server)**.

## Architektur
- DHCP-Server steht im **Server-VLAN 40**
- DHCP-Anfragen aus VLAN 10/20/30 sind Broadcasts und würden VLAN-Grenzen nicht überschreiten
- Der Router (R-CORE) übernimmt per `ip helper-address` das **DHCP Relay** (Weiterleitung an den DHCP-Server)

## IP-Plan (relevant)
- Router Gateway VLAN 10: `10.10.10.1/24`
- Router Gateway VLAN 20: `10.20.20.1/24`
- Router Gateway VLAN 30: `10.30.30.1/24`
- Router Gateway VLAN 40: `10.40.40.1/24`
- DHCP-Server: `10.40.40.10/24` (statisch)

## DHCP-Server Konfiguration (Packet Tracer)
**Server (VLAN 40):**
- IP: `10.40.40.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `10.40.40.1`

**DHCP Service:**
- Services → DHCP → **ON**
- Pools für VLAN 10/20/30 anlegen

### Beispiel-Pools (Empfehlung)
> Die genauen Start-IP/Max-Users können je nach Topologie angepasst werden.

**Pool: ADMIN (VLAN 10)**
- Default Gateway: `10.10.10.1`
- Start IP: `10.10.10.50`
- Subnet Mask: `255.255.255.0`
- DNS Server: `10.40.40.11` (oder der konfigurierte DNS)
- Max Users: z. B. `100`

**Pool: TEACHER (VLAN 20)**
- Default Gateway: `10.20.20.1`
- Start IP: `10.20.20.50`
- Subnet Mask: `255.255.255.0`
- DNS Server: `10.40.40.11`
- Max Users: z. B. `100`

**Pool: STUDENT (VLAN 30)**
- Default Gateway: `10.30.30.1`
- Start IP: `10.30.30.50`
- Subnet Mask: `255.255.255.0`
- DNS Server: `10.40.40.11`
- Max Users: z. B. `200`

## Router DHCP Relay (R-CORE)
Auf den VLAN-Subinterfaces des Routers wird der DHCP-Server als Helper gesetzt:

```text
interface g0/1.10
 ip helper-address 10.40.40.10

interface g0/1.20
 ip helper-address 10.40.40.10

interface g0/1.30
 ip helper-address 10.40.40.10
