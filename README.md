# Segmentiertes Heimnetz mit OPNsense und VLANs

> Trennung von Netzbereichen über VLANs am Managed Switch. Die Firewall läuft auf einem eigenen Rechner, getrennt vom Serverbetrieb.

**Status:** laufend. Die Detailprüfung der Switchkonfiguration ist noch offen.

## Komponenten

- **Router:** TP Link Deco x50 5G
- **Firewall:** Intel NUC mit OPNsense
- **Switch:** TP-Link Omada SG2008P (VLAN-fähig)
- **Server:** Proxmox-Host auf einem zweiten Intel NUC (siehe [Proxmox-Projekt](https://github.com/ProOstSport/homelab-proxmox))

## VLAN-Übersicht

| VLAN | Name | Hinweis |
|---|---|---|
| 1 | System | Auf allen Ports Mitglied |
| 10 | ADMIN | Administration |
| 20 | LAB | Testsysteme |
| 30 | CLIENTS | Endgeräte |
| 40 | SRV-01 | Serverbereich, Proxmox-Host und VM |
| 50 | GUEST | Gastzugang |

## Aufbau

```
OPNsense (NUC)
   │  Trunk, Switchport 7 (Mitglied in VLAN 1, 10–50)
SW-01  TP-Link Omada SG2008P
   │  Switchport 3 (VLAN 40)
Proxmox-Host srv-01 (NUC)
   └── VM docker-01 (VLAN 40)
```

## Was bereits geprüft ist

- Die Port-Zuordnung (PVID und Mitgliedschaften) wurde am Switch kontrolliert.
- Die Verbindung Proxmox-Host und VM über VLAN 40 ist nachvollzogen.
- Der Trunk zur OPNsense führt die VLANs 1 und 10 bis 50.

## Offene Punkte

- Tagged/Untagged-Zuordnung je Port prüfen
- Restliche Portbelegung erfassen
- Firewall-Regeln und DNS-Konfiguration vollständig dokumentieren
- Regelprüfung: Gezielt testen, ob Inter-VLAN-Zugriffe blockiert werden
