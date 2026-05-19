# Hardware

# EcoTech Solutions - Listing Matériel Maquette Virtuelle

Domaine : ecotech.lan  
Site : Bordeaux (BDX)  
Périmètre : 244 utilisateurs internes, 7 départements, 24 services   
Environnement : Maquette virtuelle - VMs + Packet Tracer  

## 1. Équipements réseau (Packet Tracer)

### 1.1 Équipements actifs   

| Nom | Modèle PT | Rôle | IP de gestion |
|---|---|---|---|
| FW-BDX-01 | Cisco ISR 4331 | pfSense principal - Suricata IDS/IPS - CARP actif | 172.16.80.1 |
| FW-BDX-02 | Cisco ISR 4331 | pfSense failover - Suricata IDS/IPS - CARP passif | 172.16.80.253 |
| SW-L3-BDX-01 | Cisco 3560-24PS | Switch L3 principal - Routing inter-VLAN - HSRP actif | 172.16.80.2 |
| SW-L3-BDX-02 | Cisco 3560-24PS | Switch L3 failover - Routing inter-VLAN - HSRP passif | 172.16.80.254 |
| SW-L2-BDX-01 | Cisco 2960-48TT | Switch L2 - Développement (1/3) - RADIUS 802.1X | 172.16.80.10 |
| SW-L2-BDX-02 | Cisco 2960-48TT | Switch L2 - Développement (2/3) - RADIUS 802.1X | 172.16.80.11 |
| SW-L2-BDX-03 | Cisco 2960-48TT | Switch L2 - Développement (3/3) - RADIUS 802.1X | 172.16.80.12 |
| SW-L2-BDX-04 | Cisco 2960-48TT | Switch L2 - Commercial - RADIUS 802.1X | 172.16.80.13 |
| SW-L2-BDX-05 | Cisco 2960-48TT | Switch L2 - Communication - RADIUS 802.1X | 172.16.80.14 |
| SW-L2-BDX-06 | Cisco 2960-48TT | Switch L2 - RH - RADIUS 802.1X | 172.16.80.15 |
| SW-L2-BDX-07 | Cisco 2960-48TT | Switch L2 - Finance - RADIUS 802.1X | 172.16.80.16 |
| SW-L2-BDX-08 | Cisco 2960-48TT | Switch L2 - DSI - RADIUS 802.1X | 172.16.80.17 |
| SW-L2-BDX-09 | Cisco 2960-48TT | Switch L2 - Direction - RADIUS 802.1X | 172.16.80.18 |

### 1.2 Access Points Wifi   

| Nom | Rôle | Zone |
|---|---|---|
| AP-BDX-01 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Développement |
| AP-BDX-02 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Développement |
| AP-BDX-03 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Développement |
| AP-BDX-04 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Commercial |
| AP-BDX-05 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Communication |
| AP-BDX-06 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | RH |
| AP-BDX-07 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Finance |
| AP-BDX-08 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | DSI |
| AP-BDX-09 | Access Point Wifi - WPA2 Enterprise - RADIUS 802.1X | Direction |

### 1.3 Postes clients (1 représentatif par VLAN)   

| Nom | VLAN | IP | Département |   
|---|---|---|---|
| PC-BDX-DIR-001 | 10 | 172.16.10.10 | Direction |
| PC-BDX-DSI-001 | 20 | 172.16.20.10 | DSI |
| PC-BDX-COM-001 | 30 | 172.16.30.10 | Communication |
| PC-BDX-DEV-001 | 40 | 172.16.40.10 | Développement |
| PC-BDX-RH-001 | 50 | 172.16.50.10 | RH |
| PC-BDX-FIN-001 | 60 | 172.16.60.10 | Finance |
| PC-BDX-CMR-001 | 70 | 172.16.70.10 | Commercial |

## 2. Machines virtuelles - Serveurs   

Hyperviseur : Proxmox VE (à confirmer)   

| Nom | Rôle | OS | CPU | RAM | Disque | VLAN | IP |   
|---|---|---|---|---|---|---|---|
| VM-BDX-DC-01 | Contrôleur de domaine principal - AD DS, DNS, DHCP, NPS | Windows Server 2025 Core | 2 vCPU | 4 Go | 60 Go | 80 | 172.16.80.10 |
| VM-BDX-DC-02 | Contrôleur de domaine secondaire - AD DS, DNS (redondance) | Windows Server 2025 Core | 2 vCPU | 4 Go | 60 Go | 80 | 172.16.80.11 |
| VM-BDX-FIC-01 | Serveur de fichiers - Partages réseau, DFS | Windows Server 2025 GUI | 2 vCPU | 4 Go | 100 Go | 80 | 172.16.80.20 |
| VM-BDX-BAK-01 | Serveur de sauvegarde - Bareos, backup unidirectionnel | Debian 12 | 1 vCPU | 2 Go | 200 Go | 90 | 172.16.90.10 |

## 3. Récapitulatif par département

| Département | Utilisateurs | Switches L2 | Access Points |
|---|---|---|---|
| Développement | 110 | 3 | 3 |
| Commercial | 42 | 1 | 1 |
| Communication | 38 | 1 | 1 |
| RH | 24 | 1 | 1 |
| Finance | 16 | 1 | 1 |
| DSI | 9 | 1 | 1 |
| Direction | 6 | 1 | 1 |
| **Total** | **245** | **9** | **9** |
