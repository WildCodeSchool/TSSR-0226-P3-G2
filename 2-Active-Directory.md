 Active Directory

## Contexte

La machine SRVWIN01, sous Windows Server 2022, a été promue contrôleur de domaine. Le domaine retenu est ecotechsolutions.lan, dont le nom NetBIOS est ECOTECH. Le rôle DNS s'installe automatiquement au cours de la promotion.

| Paramètre | Valeur |
|---|---|
| Nom de domaine | ecotechsolutions.lan |
| NetBIOS | ECOTECH |
| Contrôleur de domaine | SRVWIN01, en 10.0.10.10 |
| Niveau fonctionnel | Windows Server 2016 |

## Unités d'organisation

J'ai créé une unité d'organisation par département, soit sept OU à la racine du domaine : Communication, DSI, DRH, Developpement, Direction, Finance et Commercial.

<img width="628" height="301" alt="image" src="https://github.com/user-attachments/assets/2051b810-9bbc-42f0-a9c5-20717d14aacf" />

## Groupes et modèle AGDLP

Mes groupes respectent le modèle AGDLP : les comptes sont placés dans un groupe global GG, lui-même membre d'un groupe de domaine local GDL qui porte les permissions sur une ressource.

| Groupe | Etendue | Rôle |
|---|---|---|
| GG_DSI | Globale | Utilisateurs du département DSI |
| GG_Communication | Globale | Utilisateurs du département Communication |
| GG_Admins_Locaux | Globale | Administrateurs locaux des postes |
| GDL_RW_PartageDSI | Domaine local | Droits en lecture et écriture sur le partage DSI |

L'imbrication réalisée est la suivante : le groupe GG_DSI est membre de GDL_RW_PartageDSI. Elle constitue la démonstration concrète du modèle AGDLP.

<img width="397" height="148" alt="image" src="https://github.com/user-attachments/assets/c3c6dbcf-e52e-493c-b11a-abc82b3591e4" />


## Utilisateurs

J'ai créé six utilisateurs : deux managers répartis sur deux départements distincts, et quatre utilisateurs standard rattachés à un manager via l'onglet Organisation.

| Login | Nom | Département | Rôle | Responsable |
|---|---|---|---|---|
| pierre.durand | Pierre Durand | DSI | Manager | indifférent |
| lucas.bernard | Lucas Bernard | DSI | Standard | pierre.durand |
| amine.dubois | Amine Dubois | DSI | Standard | pierre.durand |
| sophie.martin | Sophie Martin | Communication | Manager | indifférent |
| lea.petit | Lea Petit | Communication | Standard | sophie.martin |
| karim.moreau | Karim Moreau | Communication | Standard | sophie.martin |

Le mot de passe initial est P@sswordInit2026!, avec changement imposé à la première connexion.

<img width="569" height="349" alt="image" src="https://github.com/user-attachments/assets/d4d8d8bf-d87a-4c12-9fa0-bb6aea077a20" />
<img width="735" height="396" alt="image" src="https://github.com/user-attachments/assets/f4eaaaf2-82c8-45a6-b2c6-e6832def6ced" />


<img width="933" height="307" alt="image" src="https://github.com/user-attachments/assets/bf2c16bf-a145-47be-842d-f4d8d3c71722" />

