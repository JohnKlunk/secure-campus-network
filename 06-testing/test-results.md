# Network Testing & Verification

Dieses Dokument beschreibt die durchgeführten Tests zur Überprüfung der
Funktionalität, Sicherheit und Stabilität des Campus-Netzwerks.

---

## 1. DHCP-Tests

### Ziel
Überprüfen, ob Clients in den jeweiligen VLANs automatisch IP-Adressen
vom zentralen DHCP-Server erhalten.

### Durchführung
- Client-PCs in VLAN 10 (Admin), VLAN 20 (Teacher) und VLAN 30 (Student)
  wurden auf DHCP gestellt.
- DHCP-Server befindet sich im Server-VLAN (VLAN 40).
- DHCP-Relay (`ip helper-address`) ist auf den Router-Subinterfaces konfiguriert.

### Ergebnis
| VLAN | Erwartet | Ergebnis |
|-----|----------|---------|
| VLAN 10 (Admin) | IP aus 10.10.10.0/24 | Erfolgreich |
| VLAN 20 (Teacher) | IP aus 10.20.20.0/24 | Erfolgreich |
| VLAN 30 (Student) | IP aus 10.30.30.0/24 | Erfolgreich |

---

## 2. Gateway-Erreichbarkeit

### Ziel
Sicherstellen, dass alle Clients ihr jeweiliges Default-Gateway erreichen.

### Durchführung
Ping vom Client zum Gateway des eigenen VLANs.

### Ergebnis
| VLAN | Gateway | Ping |
|-----|--------|------|
| VLAN 10 | 10.10.10.1 |
| VLAN 20 | 10.20.20.1 |
| VLAN 30 | 10.30.30.1 |

---

## 3. Inter-VLAN-Routing

### Ziel
Überprüfen, ob Routing zwischen VLANs grundsätzlich funktioniert.

### Durchführung
Ping zwischen Clients verschiedener VLANs (ohne ACL-Einschränkung).

### Ergebnis
- Routing zwischen VLANs funktioniert technisch korrekt.
- Einschränkungen erfolgen ausschließlich über ACLs.

---

## 4. ACL-Test – Student VLAN (VLAN 30)

### Ziel
Sicherstellen, dass Student-Clients eingeschränkt sind:
- Kein Zugriff auf Admin- und Teacher-Netze
- Zugriff auf Webserver erlaubt
- Kein Ping auf Server erlaubt

### Durchführung
Tests von einem Client im VLAN 30.

### Ergebnis
| Ziel | Erwartung | Ergebnis |
|-----|----------|---------|
| Ping Admin (10.10.10.0/24) | Blockiert |
| Ping Teacher (10.20.20.0/24) | Blockiert |
| Ping Server (10.40.40.20) | Blockiert | 
| HTTP Webserver (10.40.40.20) | Erlaubt |

---

## 5. ACL-Test – Server Protection

### Ziel
Absicherung des Servers:
- Nur HTTP/HTTPS erlaubt
- Kein ICMP-Zugriff
- Kein unkontrollierter Netzwerkzugriff

### Durchführung
Zugriffstests aus verschiedenen VLANs.

### Ergebnis
| Protokoll | Zugriff |
|----------|--------|
| HTTP (Port 80) | Erlaubt |
| HTTPS (Port 443) | Erlaubt |
| ICMP (Ping) | Blockiert |
| Sonstiger IP-Traffic | Blockiert |

---

## 6. Management-Zugriff (SSH)

### Ziel
Sicherstellen, dass Netzwerkgeräte nur aus dem Admin-VLAN verwaltet werden können.

### Durchführung
- SSH-Zugriff von VLAN 10 (Admin)
- SSH-Zugriff von VLAN 20 / 30

### Ergebnis
| Quelle | Ergebnis |
|------|---------|
| VLAN 10 (Admin) | Zugriff möglich |
| VLAN 20 (Teacher) | Zugriff verweigert |
| VLAN 30 (Student) | Zugriff verweigert |

---

## 7. Fazit

Alle Kernfunktionen des Netzwerks wurden erfolgreich getestet:

- VLAN-Struktur funktioniert stabil
- DHCP mit zentralem Server und Relay arbeitet korrekt
- Routing ist sauber implementiert
- Sicherheitsrichtlinien via ACL greifen wie geplant
- Server und Management-Zugänge sind abgesichert

Das Netzwerk ist funktionsfähig, sicher und bereit für Dokumentation
und Präsentation im GitHub-Portfolio.
