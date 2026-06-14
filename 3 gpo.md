# Objectif 3. Stratégies de groupe GPO

## Contexte

J'ai créé sept GPO distinctes, toutes liées au domaine ecotechsolutions.lan. Elles couvrent les cinq politiques imposées par le sujet, complétées par deux politiques au choix : le renommage du compte administrateur et le blocage du stockage USB.

CAPTURE A INSERER : la console GPMC présentant les sept GPO liées au domaine
Fichier : Ressources/00-gpmc-7-gpo.png

## GPO 1. GPO_DOMAINE_POLICEMDP

Chemin : Configuration ordinateur, Stratégies, Paramètres Windows, Paramètres de sécurité, Stratégies de comptes, Stratégie de mot de passe.

| Paramètre | Valeur |
|---|---|
| Historique des mots de passe | 5 |
| Durée de vie maximale | 30 jours |
| Durée de vie minimale | 1 jour |
| Longueur minimale | 14 caractères |
| Exigences de complexité | Activé |
| Chiffrement réversible | Désactivé |

Cette GPO est liée à la racine du domaine, condition indispensable pour qu'elle s'applique à l'ensemble des comptes du domaine.

<img width="1206" height="464" alt="image" src="https://github.com/user-attachments/assets/b9fa1b06-7390-492b-bc23-ed6c17a0e2cd" />


Capture déjà fournie.

## GPO 2. GPO_DOMAINE_VERRCOMPTE

Chemin : Stratégies de comptes, Stratégie de verrouillage du compte.

| Paramètre | Valeur |
|---|---|
| Seuil de verrouillage | 5 tentatives |
| Durée de verrouillage | 15 minutes |
| Réinitialisation du compteur après | 15 minutes |

<img width="1223" height="451" alt="image" src="https://github.com/user-attachments/assets/eaab266f-3835-4c9e-a12c-53af987edb38" />


Capture déjà fournie.

## GPO 3. GPO_BLOCAGEPANNEAUCONFIG

Chemin : Configuration utilisateur, Stratégies, Modèles d'administration, Panneau de configuration.

| Paramètre | Valeur |
|---|---|
| Interdire l'accès au Panneau de configuration et aux paramètres du PC | Activé |

<img width="1164" height="434" alt="image" src="https://github.com/user-attachments/assets/2447a185-577f-4044-a5df-b502bd06c0f4" />


Capture déjà fournie.

## GPO 4. GPO_COMPUTERS_ADMINLOCAL

Chemin : Configuration ordinateur, Préférences, Paramètres du Panneau de configuration, Utilisateurs et groupes locaux. Je mets à jour le groupe Administrateurs intégré afin d'y ajouter GG_Admins_Locaux. Les membres de ce groupe deviennent ainsi administrateurs locaux des postes.

<img width="1155" height="541" alt="image" src="https://github.com/user-attachments/assets/9298052e-22f7-4821-b448-700172ed0726" />


Capture fournie présentant la console GPMC et la fenêtre Utilisateurs et groupes locaux. Pour un rendu optimal, une capture en plein écran de la fenêtre Administrateurs intégré faisant apparaître GG_Admins_Locaux serait préférable.

## GPO 5. GPO_COMPUTERS_POWERSHELLSECURITY

Chemin : Configuration ordinateur, Stratégies, Modèles d'administration, Composants Windows, Windows PowerShell.

| Paramètre | Valeur |
|---|---|
| Activer l'exécution des scripts | Activé, scripts locaux et signés distants |
| Journalisation des modules | Activé |
| Journalisation des blocs de scripts | Activé |
| Transcription PowerShell | Activé |

<img width="1200" height="481" alt="image" src="https://github.com/user-attachments/assets/45f04b5e-f3af-4a23-a122-d37b19fc4e30" />


Capture déjà fournie.

## GPO 6. GPO_COMPUTERS_RENOMMERADMIN

Chemin : Configuration ordinateur, Stratégies, Paramètres Windows, Paramètres de sécurité, Stratégies locales, Options de sécurité.

| Paramètre | Valeur |
|---|---|
| Renommer le compte administrateur | Eco_SuperviseurRoot |
| Renommer le compte Invité | Eco_InviteOff |
| Statut du compte Invité | Désactivé |

<img width="1228" height="494" alt="image" src="https://github.com/user-attachments/assets/ee62bc5c-05f7-408a-8b8a-5c7d0b2a44f0" />


Capture déjà fournie.

## GPO 7. GPO_COMPUTERS_BLOCAGEUSBSTORAGE

Chemin : Configuration ordinateur, Stratégies, Modèles d'administration, Système, Accès au stockage amovible.

| Paramètre | Valeur |
|---|---|
| Disques amovibles, refuser l'accès en lecture | Activé |
| Disques amovibles, refuser l'accès en écriture | Activé |

<img width="1121" height="589" alt="image" src="https://github.com/user-attachments/assets/6bf9283a-e87a-4c87-b3d4-38587ed45ff3" />


Capture déjà fournie.

## Vérification

```
gpupdate /force
gpresult /r
```

<img width="832" height="373" alt="image" src="https://github.com/user-attachments/assets/371723c1-4304-4c5e-a5bc-80df28dbd78f" />
