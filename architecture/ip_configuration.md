# ip_configuration


# I. Plan d'adressage IP:

## Logique de nommage

```
172.16.[Zone].[Hôte]
```

| Octet | Rôle |
|---|---|
| 1er | Réseau racine (RFC 1918) |
| 2ème| Organisation EcoTech Solutions |
| 3ème | Zone / VLAN |
| 4ème | Hôte / Machine |

---

## II. VLANs et plages d'adresses:

| VLAN | Zone | Réseau | Passerelle | Plage hôtes | Masque |
|---|---|---|---|---|---|
| VLAN 10 | Direction | 172.16.10.0 | 172.16.10.1 | .2 - .254 | /24 |
| VLAN 20 | DSI | 172.16.20.0 | 172.16.20.1 | .2 -.254 | /24 |
| VLAN 30 | Communication | 172.16.30.0 | 172.16.30.1 | .2 - .254 | /24 |
| VLAN 40 | Développement | 172.16.40.0 | 172.16.40.1 | .2 - .254 | /24 |
| VLAN 50 | RH | 172.16.50.0 | 172.16.50.1 | .2 - .254 | /24 |
| VLAN 60 | Finance & Comptabilité | 172.16.60.0 | 172.16.60.1 | .2 - .254 | /24 |
| VLAN 70 | Service Commercial | 172.16.70.0 | 172.16.70.1 | .2 - .254 | /24 |
| VLAN 80 | Serveurs | 172.16.80.0 | 172.16.80.1 | .2 - .254 | /24 |
| VLAN 90 | Backup (non routé) | 172.16.90.0 | 172.16.90.1 | .2 - .254 | /24 |
| VLAN 100 | Périphériques | 172.16.100.0/24 | 172.16.100.1 | .2 - .254 | /24 |

---

## III. Adresses fixes des serveurs:

### VLAN 80 - Serveurs:

| Serveur | IP fixe | Rôles |
|---|---|---|
| DC1 | 172.16.80.10 | AD DS principal, DNS, DHCP, NPS/RADIUS |
| DC2 | 172.16.80.11 | AD DS secondaire, DNS secondaire |
| Serveur de fichiers | 172.16.80.20 | Partages réseau par département |
| pfSense (LAN) | 172.16.80.1 | Firewall, routeur, Suricata IDS/IPS |

### VLAN 90 - Backup:

| Serveur | IP fixe | Rôles |
|---|---|---|
| Bareos | 172.16.90.10 | Sauvegarde centralisée (non routé) |

---

## IV. Règles d'attribution:

- `.1` - Passerelle (pfSense / Switch L3)
- `.2 / .19` - Équipements réseau (switchs, AP...)
- `.10 / .99` - Serveurs (IPs fixes)
- `.100 / .254` - Hôtes utilisateurs (DHCP)

---

## V. Externe:

| Entité | Statut |
|---|---|
| Studio Dlight - Nantes | Hors réseau EcoTech |
