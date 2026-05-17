# Hardware
# EcoTech Solutions - Listing Matériel Maquette Virtuelle

> **Domaine** : `ecotech.lan`
> **Site** : Bordeaux (BDX)
> **Périmètre** : 243 utilisateurs internes, 7 départements, 24 services
> **Environnement** : Maquette virtuelle - VMs + Packet Tracer

---

## 1. Équipements réseau (Packet Tracer)

### 1.1 Équipements actifs
| Nom | Modèle Packet Tracer | Rôle | IP de gestion |
|------|---------------------|------|----------------|
|FW-BDX-01 | Routeur Cisco 4331 | Pare-feu, NAT, routage WAN | 172.16.1.1 |
| SW-L3-BDX-01 | Switch Cisco 3650 | Routing inter-VLAN, agrégation | 172.16.1.10 |
| SW-L2-BDX-01 | Switch Cisco 2960 | Accès utilisateurs, 802.1X | 172.16.1.11 |

## 1.2 Postes clients (1 par VLAN)

| Nom | VLAN | IP | Département |
|------|-----|----|------------|
| PC-BDX-DIR-001 | 10 | 172.16.10.10| Direction |
|PC-BDX-DSI-001 | 20 | 172.16.20.10 | DSI |
| PC-BDX-COM-001 | 30 | 172.16.30.10 | Communication |
| PC-BDX-DEV-001 | 40 | 172.16.40.10 | Développement |
| PC-BDX-RH-001 | 50 | 172.16.50.10 | RH |
| PC-BDX-FIN-001 | 60 | 172.16.60.10 | Finance |
| PC-BDX-CMR-001 | 70 | 172.16.70.10 | Commercial|

---

## 2. Machines virtuelles - Serveurs 

| Nom | Rôle | OS | CPU | RAM | Disque | VLAN | IP |
|-----|------|----|-----|-----|--------|------|----|
| VM-BDX-DC-01 | Controleur de domaine principal - AD DS, DNS, DHCP, NPS | Windows Server Core 2025 | 2 | 4 Go | 60 Go | 80 | 172.16.80.10 |
| VM-BDX-DC-02 | Controleur de domaine secondaire - AD DS, DNS (rebondance) | Windows Server Core 2025 | 2 | 4 Go | 60 Go| 80 | 172.16.80.11 |
| VM-BDX-FIC-01 | Sauvegarde de fichiers - Partages réseau, DFS | Windows Servers 2025 | 2 | 4 Go| 100 Go | 80 | 172.16.80.20 |
| VM-BDX-BAK-01 | Serveur de sauvegarde - BareOS, backup unidirectionnel | Debian 13 | 1 | 2 Go | 200 Go | 90 | 172.16.90.10 |

---
