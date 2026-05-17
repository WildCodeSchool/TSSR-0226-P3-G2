# NOmenclature EcoTech Solutions

# I. Nom de domaine
Nom FQDN : ecotech.lan

# II. Unités d'Organisation (OU)
**Structure hiérarchique par type d'objet puis par département.**

## Racine : ecotech.lan/EcoTech

- **Utilisateurs**
  - Direction
  - DSI
  - Communication
  - Developpement
  - RH
  - Finance
  - Commercial
    
- **Ordinateurs**
  - Direction
  - DSI
  - Communication
  - Developpement
  - RH
  - Finance
  - Commercial
    
- **Serveurs**
  - DC
  - Fichiers
    
- **Groupes**
  - Securite
  - Distribution



# III. Groupes de sécurité

**Convention AGDLP (Account - Global - Domain Local - Permission).**

## Groupes Globaux (G) - regroupent les utilisateurs par département :

- G-Direction-Utilisateurs
- G-DSI-Utilisateurs
- G-Communication-Utilisateurs
- G-Developpement-Utilisateurs
- G-RH-Utisateurs
- G-Finance-Utisateurs
- G-Commercial-Utisateurs
- G-DSI-Admins

## Groupes Domain Local (DL) - attachés aux ressources :

- DL-PartageDirection-RO
- DL-PartageDirection-RW
- DL-PartageRH-RO
- DL-PartageRH-RW
- DL-PartageFinance-RO
- DL-PartageFinance-RW
- DL-PartageCommercial-RO
- DL-PartageCommercial-RW
- DL-PartageDevEloppement-RO
- DL-PartageDevEloppement-RW
- DL-PartageGlobal-RO

## Légende droits :

RO : Read Only (lecture seule)
RW : Read/Write (lecture et écriture)

# IV. Ordinateurs

| Type | Signification |
|---|---|
| BAK | Serveur de backup |
| BDX | Site de Bordeaux |
| CMR | Commercial |
| COM | Communication |
| DC | Domain Controller |
| DEV | Développement |
| DIR | Direction |
| DSI | DSI |
| FIC | Serveur de fichiers |
| FIN | Fina'nce |
| PC | Ordinateur client |
| RH | Ressources Humaines |
| SRV | Serveur physique |
| VM | Machine virtuelle |

Exemples :

PC-BDX-DIR-001
PC-BDX-RH-042
SRV-BDX-DC-01
SRV-BDX-DC-02
SRV-BDX-FIC-01
SRV-BDX-BAK-01
VM-BDX-DC-01
VM-BDX-FIC-01

Numérotation sur 3 chiffres : 001, 002, 003...

# V. Utilisateurs

Convention : prenom.nom
Exemples :

grégory.datis
zinédine.zishan
micheal.scott

Gestion des homonymes : ajout d'un chiffre en suffixe.

dunder.milfin
dunder.milfin2

Comptes administrateurs : adm.[prenom]

adm.zishan
adm.gregory
adm.zinedine

# VI. GPO

| Abréviation | Signification |
|---|---|
| ALL | Tout le domaine |
| CFG | Configuration |
| PC | S'applique aux ordinateurs |
| REST | Restriction |
| SEC | Sécurité |
| USR | S'applique aux utilisateurs |

Exemples :

PC-SEC-BitLocker-ALL  
PC-SEC-LAPS-ALL  
PC-CFG-WallPaper-ALL  
PC-REST-USB-ALL  
USR-SEC-MDP-ALL  
USR-REST-PanneauConfig-ALL  
USR-CFG-Drive-RH  
USR-CFG-Drive-DEV  



### Naming
