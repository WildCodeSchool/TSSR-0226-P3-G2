# Objectif 1 — Pare-feu pfSense

## Contexte

Machine : **FW01**. Pare-feu pfSense CE 2.7.2, trois interfaces : WAN (sortie internet), LAN (réseau interne, console web sur 10.0.10.254), DMZ. Principe appliqué : **Deny All** — tout est bloqué par défaut, seuls les flux explicitement autorisés passent.

| Interface | Réseau | Rôle |
|---|---|---|
| WAN | DHCP (NAT VMware) | Sortie internet, simulation box FAI |
| LAN | 10.0.10.0/24 — passerelle 10.0.10.254 | Réseau interne |
| DMZ | 10.0.20.0/24 — passerelle 10.0.20.254 | Zone exposée |

## Configuration WAN

Les options **Block private networks** et **Block bogon networks** sont décochées (le WAN reçoit une IP privée du NAT VMware ; les laisser actives bloquerait la sortie). Aucune règle entrante n'est définie : tout le trafic entrant est bloqué par défaut, ce qui correspond au Deny All natif.

<img width="694" height="207" alt="image" src="https://github.com/user-attachments/assets/abcf98c2-7606-4d3d-94f9-364539b93014" />

_Onglet WAN : aucune règle entrante (Deny All natif) et Reserved Networks décochés. **Capture déjà fournie.**_

## Règles LAN

| Ordre | Action | Protocole | Source | Destination | Port | Rôle |
|---|---|---|---|---|---|---|
| auto | Pass | — | * | LAN Address | 80 | Anti-Lockout (accès web GUI) |
| 1 | Pass | TCP | LAN subnets | any | 443 | Sortie HTTPS |
| 2 | Pass | TCP | LAN subnets | any | 80 | Sortie HTTP |
| 3 | Pass | ICMP | LAN subnets | any | — | Ping / diagnostic |
| 4 | Block | any | LAN subnets | DMZ subnets | — | Le LAN ne parle pas à la DMZ |
| 5 | Pass | TCP/UDP | LAN subnets | any | 53 | Résolution DNS |
| bas | Pass | — | LAN subnets | any | * | Default allow — **désactivée** |

La désactivation de « Default allow LAN to any » active la règle implicite de blocage : tout flux non autorisé est rejeté. C'est la mise en œuvre du Deny All.

<img width="717" height="401" alt="image" src="https://github.com/user-attachments/assets/b3bf0940-b7dc-487d-a088-c263e0e07c0d" />

_Règles LAN, Default allow désactivée. **Capture déjà fournie.**_

### Tests de validation (depuis SRVWIN01)

| Commande | Résultat attendu |
|---|---|
| `ping 10.0.10.254` | Réponse de la passerelle |
| `ping 8.8.8.8` | Sortie internet OK |
| `nslookup google.com` | Résolution externe OK |

## Règles DMZ

| Ordre | Action | Protocole | Source | Destination | Port | Rôle |
|---|---|---|---|---|---|---|
| 1 | Block | any | DMZ subnets | LAN subnets | — | La DMZ ne touche jamais le LAN |
| 2 | Pass | UDP | DMZ subnets | This Firewall | 53 | DNS |
| 3 | Pass | TCP | DMZ subnets | any | 80 | Sortie HTTP |
| 4 | Pass | TCP | DMZ subnets | any | 443 | Sortie HTTPS |

La règle de blocage DMZ→LAN est en première position, donc évaluée avant les règles de sortie : un serveur DMZ compromis reste cloisonné.

<img width="718" height="288" alt="image" src="https://github.com/user-attachments/assets/942e82e3-d77e-4eba-b6f2-7c3355e1d72a" />

_Règles DMZ, blocage DMZ→LAN en tête. **Capture déjà fournie.**_

## Vérification finale

État final : LAN en Deny All actif, DMZ cloisonnée, WAN en blocage entrant natif. Objectif 1 terminé.
