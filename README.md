# Projet d'Infrastructure Réseau EcoTech Solutions

## Sommaire

1. [Présentation du Projet](#1-présentation-du-projet)
2. [Nomenclature et Convention de Nommage](#2-nomenclature-et-convention-de-nommage)
3. [Plan d'Adressage IP et Segmentation Réseau](#3-plan-dadressage-ip-et-segmentation-réseau)
4. [Architecture de l'Annuaire Active Directory (OU)](#4-architecture-de-lannuaire-active-directory-ou)
5. [Liste du Matériel et Configuration des VM](#5-liste-du-matériel-et-configuration-des-vm)
6. [Schéma Réseau de l'Infrastructure](#6-schéma-réseau-de-linfrastructure)
7. [Calcul du Temps de Projet (ETP)](#7-calcul-du-temps-de-projet-etp)
8. [Journal de Bord et Suivi de Projet](#8-journal-de-bord-et-suivi-de-projet)

---

## 1. Présentation du Projet

### Contexte

**EcoTech Solutions** est une entreprise basée à Bordeaux, spécialisée dans les solutions IoT dédiées à la gestion intelligente de l'énergie et des ressources. Elle emploie **245 collaborateurs** répartis au sein de **7 départements**.

### Situation actuelle (état des lieux)

L'infrastructure existante présente de nombreuses faiblesses :
- Réseau Wi-Fi via box FAI et répéteurs, sans segmentation
- Aucun serveur ni matériel réseau dédié
- Tous les postes en **workgroup** avec des mots de passe locaux réutilisés
- Stockage de données en local sur les machines, **aucune sauvegarde**
- Aucune sécurité d'identité centralisée
- Téléphonie non standardisée (téléphones personnels)
- Pas de nomadisme ni de télétravail

### Objectif du projet

Concevoir et déployer une **infrastructure réseau centralisée, sécurisée et managée**, comprenant :
- Un pare-feu avec segmentation WAN / LAN / DMZ
- Un domaine Active Directory avec gestion centralisée des identités
- Des services réseau (DNS, DHCP, WSUS)
- Un outil de gestion de parc et ticketing (GLPI)
- Une solution de téléphonie VoIP (FreePBX)
- Une messagerie interne (Zimbra ou iRedMail)

### Départements de l'entreprise

| Département | Effectif |
|---|---|
| Développement | 110 |
| Service Commercial | 42 |
| Communication | 38 |
| Direction des Ressources Humaines | 24 |
| Finance et Comptabilité | 16 |
| DSI | 9 |
| Direction | 6 |
| **Total** | **245** |

---

## 2. Nomenclature et Convention de Nommage

### 2.1. Domaine

| Élément | Valeur |
|---|---|
| Nom de domaine AD | `tssr.lan` |
| Nom FQDN du DC principal | `SRVWIN01.tssr.lan` |

### 2.2. Serveurs et équipements réseau

| Nom | Rôle |
|---|---|
| `FW01` | Pare-feu pfSense |
| `SRVWIN01` | Contrôleur de domaine principal (AD DS, DNS, DHCP) |
| `SRVWIN04` | Serveur de mises à jour (WSUS) |
| `SRVLX01` | Serveur Linux  GLPI et Messagerie |
| `IPBX01` | Serveur VoIP  FreePBX |

### 2.3. Postes clients

| Nom | OS |
|---|---|
| `CLIWIN01` | Windows 10 Pro |
| `CLIWIN02` | Windows 11 Pro |

### 2.4. Utilisateurs Active Directory

- **Format de login** : `<2 premières lettres du prénom><nom>` en minuscules, sans accent ni espace
- **Exemple** : Zinedine Balamane → `zibalamane`
- **Gestion des homonymes** : ajout d'un chiffre incrémental (`zibalamane1`, `zibalamane2`)

### 2.5. Format de messagerie

- **Format** : `<2 premières lettres du prénom><nom>@ecotechsolutions.fr`
- **Exemple** : Zinedine Balamane → `zibalamane@ecotechsolutions.fr`

### 2.6. Groupes Active Directory (AGDLP)

| Type | Convention de nommage | Exemple |
|---|---|---|
| Groupe Global (GG) | `GG_<Département>` | `GG_DSI` |
| Groupe Domaine Local (GDL) | `GDL_<Ressource>_<Droits>` | `GDL_Partage_DSI_RW` |

### 2.7. GPO

- **Format** : `GPO_<Cible>_<Fonction>`
- `<Cible>` : `U` (utilisateur) ou `C` (ordinateur)
- **Exemples** : `GPO_C_PolitiqueMdp`, `GPO_U_BlocagePanneauConfig`

### 2.8. Unités d'Organisation (OU)

- **Format** : `OU_<Nom>`
- **Exemples** : `OU_Utilisateurs`, `OU_DSI`, `OU_Ordinateurs`

---

## 3. Plan d'Adressage IP et Segmentation Réseau

### 3.1. Vue d'ensemble

| Zone | Réseau | Rôle |
|---|---|---|
| WAN | IP via DHCP (box FAI) | Accès Internet |
| LAN | `10.0.10.0/24` | Réseau interne sécurisé |
| DMZ | `10.0.20.0/24` | Zone exposée (serveurs web) |

### 3.2. Zone LAN - `10.0.10.0/24`

**Passerelle par défaut** : `10.0.10.254` (interface LAN du pfSense)

#### Plage serveurs (IP fixes) : `10.0.10.1` — `10.0.10.50`

| Machine | Adresse IP | DNS | Passerelle |
|---|---|---|---|
| `SRVWIN01` | `10.0.10.10` | `127.0.0.1` | `10.0.10.254` |
| `SRVWIN04` | `10.0.10.12` | `10.0.10.10` | `10.0.10.254` |
| `SRVLX01` | `10.0.10.20` | `10.0.10.10` | `10.0.10.254` |
| `IPBX01` | `10.0.10.30` | `10.0.10.10` | `10.0.10.254` |

#### Plage clients (DHCP) : `10.0.10.100` - `10.0.10.200`

| Machine | Adresse IP | DNS | Passerelle |
|---|---|---|---|
| `CLIWIN01` | DHCP | `10.0.10.10` | `10.0.10.254` |
| `CLIWIN02` | DHCP | `10.0.10.10` | `10.0.10.254` |

### 3.3. Zone DMZ - `10.0.20.0/24`

**Passerelle par défaut** : `10.0.20.254` (interface DMZ du pfSense)

| Machine | Adresse IP | Rôle |
|---|---|---|
| Serveur Web (futur) | `10.0.20.10` | Hébergement web externe |

---

## 4. Architecture de l'Annuaire Active Directory (OU)

Structure hiérarchique sous la racine du domaine `tssr.lan` :

```
tssr.lan
└── OU_EcoTech
    ├── OU_Utilisateurs
    │   ├── OU_Direction
    │   ├── OU_DSI
    │   ├── OU_Communication
    │   ├── OU_Developpement
    │   ├── OU_ServiceCommercial
    │   ├── OU_RH
    │   └── OU_FinanceComptabilite
    ├── OU_Ordinateurs
    ├── OU_Serveurs
    └── OU_Groupes
```

### Utilisateurs à créer (minimum requis : 6)

| Prénom | Nom | Département | Fonction | Login AD |
|---|---|---|---|---|
| Wassim | Ibrahimovic | Communication | Directeur Communication | `waibrahimovic` |
| Hugo | Dimitrov | Développement | Responsable Développement | `hudimitrov` |
| Yassine | Ben Salah | Communication | Responsable Communication | `yabensalah` |
| Ali | Al-Khalil | Communication | Chargée relation presse | `alal-khalil` |
| Dina | Abdeslam | Développement | Développeur frontend | `diabdeslam` |
| Sakura | Abe | Développement | Développeur backend | `saabe` |

> **2 managers** (Wassim Ibrahimovic, Hugo Dimitrov) dans 2 départements différents + **4 utilisateurs standards** rattachés à ces managers.

### GPO à implémenter (8 minimum)

| Nom GPO | Cible | Description |
|---|---|---|
| `GPO_C_PolitiqueMdp` | Ordinateurs | Complexité, longueur minimale, expiration du mot de passe |
| `GPO_C_VerrouillagCompte` | Ordinateurs | Blocage après X tentatives échouées |
| `GPO_U_BlocagePanneauConfig` | Utilisateurs | Restriction d'accès au panneau de configuration |
| `GPO_C_AdminLocal` | Ordinateurs | Compte du domaine administrateur local des machines |
| `GPO_C_SecuritePowerShell` | Ordinateurs | Politique d'exécution PowerShell |
| `GPO_U_FondEcran` | Utilisateurs | Fond d'écran d'entreprise EcoTech Solutions |
| `GPO_U_MappageLecteur` | Utilisateurs | Mappage automatique des lecteurs réseau |
| `GPO_C_PareFeuxWindows` | Ordinateurs | Activation et configuration du pare-feu Windows |

---

## 5. Liste du Matériel et Configuration des VM

| Nom VM | Rôle / OS | Interfaces réseau | Paramètres IP | Identifiants |
|---|---|---|---|---|
| **FW01** | pfSense (Routage / Pare-feu) | Adaptateur 1 : WAN (Pont/Bridge) — Adaptateur 2 : LAN (Réseau interne) — Adaptateur 3 : DMZ (Réseau interne) | WAN : DHCP — LAN : `10.0.10.254/24` — DMZ : `10.0.20.254/24` | `admin` / `pfsense` |
| **SRVWIN01** | Windows Server 2022 GUI — AD DS, DNS, DHCP | Adaptateur 1 : LAN (Réseau interne) | `10.0.10.10/24` — GW : `10.0.10.254` — DNS : `127.0.0.1` | `Administrator` / `Azerty1*` |
| **SRVWIN04** | Windows Server 2022 GUI — WSUS | Adaptateur 1 : LAN (Réseau interne) | `10.0.10.12/24` — GW : `10.0.10.254` — DNS : `10.0.10.10` | `Administrator` / `Azerty1*` |
| **SRVLX01** | Debian 12 CLI — GLPI + Messagerie | Adaptateur 1 : LAN (Réseau interne) | `10.0.10.20/24` — GW : `10.0.10.254` — DNS : `10.0.10.10` | `root` / `Azerty1*` |
| **IPBX01** | FreePBX — VoIP | Adaptateur 1 : LAN (Réseau interne) | `10.0.10.30/24` — GW : `10.0.10.254` — DNS : `10.0.10.10` | `root` / `Azerty1*` |
| **CLIWIN01** | Windows 10 Pro | Adaptateur 1 : LAN (Réseau interne) | DHCP (`10.0.10.100` — `10.0.10.200`) | `wilder` / `Azerty1*` |
| **CLIWIN02** | Windows 11 Pro | Adaptateur 1 : LAN (Réseau interne) | DHCP (`10.0.10.100` — `10.0.10.200`) | `wilder` / `Azerty1*` |

---

## 6. Schéma Réseau de l'Infrastructure

> ⚠️ Le schéma réseau complet est disponible dans le dossier [`Ressources/`](./Ressources/).
> Un schéma global (Draw.io) et un schéma réseau (Packet Tracer) seront ajoutés.

```
                        ┌──────────────┐
                        │   INTERNET   │
                        └──────┬───────┘
                               │ WAN (DHCP)
                        ┌──────┴───────┐
                        │    FW01      │
                        │   pfSense    │
                        └──┬───────┬───┘
                  LAN      │       │     DMZ
            10.0.10.0/24   │       │   10.0.20.0/24
           ┌───────────────┘       └──────────────┐
           │                                      │
    ┌──────┴──────┐                        ┌──────┴──────┐
    │  SWITCH LAN │                        │  SWITCH DMZ │
    └──┬──┬──┬──┬─┘                        └──────┬──────┘
       │  │  │  │                                 │
       │  │  │  └──── IPBX01 (.30)         Serveur Web
       │  │  └─────── SRVLX01 (.20)        (futur)
       │  └────────── SRVWIN04 (.12)
       └───────────── SRVWIN01 (.10)
                           │
                    ┌──────┴──────┐
                    │  DHCP Pool  │
                    │ .100 — .200 │
                    └──┬──────┬───┘
                       │      │
                 CLIWIN01  CLIWIN02
```

---

## 7. Calcul du Temps de Projet (ETP)

### Contraintes

| Élément | Valeur |
|---|---|
| Nombre de personnes | 1 (projet individuel) |
| Durée du projet | 10 semaines |
| Heures estimées par semaine | ~10-12h |
| Capacité totale estimée | ~100-120h |

### Répartition prévisionnelle

| Brique technique | Charge estimée (JH) |
|---|---|
| Sprint 1 Documentation, schéma, nomenclature | 1 JH |
| Pare-feu pfSense (FW01) | 0.5 JH |
| AD DS + DNS + DHCP (SRVWIN01) | 1.5 JH |
| Clients Windows (CLIWIN01, CLIWIN02) | 0.5 JH |
| GLPI (SRVLX01) | 0.5 JH |
| WSUS (SRVWIN04) | 0.5 JH |
| VoIP FreePBX (IPBX01) | 1 JH |
| Messagerie (SRVLX01) | 1 JH |
| Documentation technique (INSTALL + USER_GUIDE) | 1 JH |
| **Total** | **~8 JH** |

> 1 JH = 1 personne × 1 jour (7h). Total ≈ 56h de travail effectif.

---

## 8. Journal de Bord et Suivi de Projet

### Choix techniques

| Élément | Choix |
|---|---|
| Hyperviseur | Oracle VirtualBox |
| OS Serveur Windows | Windows Server 2022 |
| OS Serveur Linux | Debian 12 CLI |
| Pare-feu | pfSense |
| GLPI | Dernière version stable |
| VoIP | FreePBX |
| Messagerie | iRedMail ou Zimbra |
| Schéma réseau | Draw.io + Packet Tracer |

### Difficultés rencontrées



### Solutions apportées



### Améliorations possibles


