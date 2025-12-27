# Intranet Webserver (VLAN 40)

## Ziel
Ein zentraler Intranet-Webserver stellt eine interne Website bereit, die von Admin/Teacher/Student erreichbar ist.

## Webserver
- IP: **10.40.40.20**
- Standort: **VLAN 40 (Server)**
- Gateway: `10.40.40.1`

## Zugriffskonzept (Security)
Der Zugriff wird über ACLs gesteuert:
- **HTTP (Port 80)** ist für bestimmte VLANs erlaubt
- **ICMP/Ping** wurde (für Student zumindest) blockiert, damit der Webserver nicht „pingbar“ ist, aber die Website trotzdem erreichbar bleibt

### Wichtiger Punkt
Dass „HTTP funktioniert, Ping nicht“ ist **gewollt**, wenn ICMP per ACL geblockt wird.
Das ist eine typische Härtungsmaßnahme: Service-Zugriff erlauben, unnötige Protokolle blocken.

## Tests / Validierung
### Web-Zugriff
1. Client (z. B. aus VLAN 10/20/30) → Browser öffnen
2. URL:
   - `http://10.40.40.20`

Erwartung:
- Seite lädt (HTTP erlaubt)

### Ping-Verhalten (je nach ACL)
- `ping 10.40.40.20` kann (und soll teilweise) fehlschlagen, je nach Regelwerk
- Für Student-VLAN war das Ziel: **Web erreichbar, Ping blockiert**

## Troubleshooting (kurz)
Wenn Web nicht geht:
- Prüfen: Webservice auf Server aktiviert (Services → HTTP → ON)
- Prüfen: Default Gateway am Server korrekt (`10.40.40.1`)
- Prüfen: ACL-Reihenfolge (permit für tcp/80 muss vor deny stehen)
- Prüfen: ACL ist am richtigen Interface/Direction gebunden
