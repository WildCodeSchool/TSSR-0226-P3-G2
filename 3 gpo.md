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

![GPO mot de passe](Ressources/01-gpo-mdp.png)

Capture déjà fournie.

## GPO 2. GPO_DOMAINE_VERRCOMPTE

Chemin : Stratégies de comptes, Stratégie de verrouillage du compte.

| Paramètre | Valeur |
|---|---|
| Seuil de verrouillage | 5 tentatives |
| Durée de verrouillage | 15 minutes |
| Réinitialisation du compteur après | 15 minutes |

![GPO verrouillage](Ressources/02-gpo-verrouillage.png)

Capture déjà fournie.

## GPO 3. GPO_BLOCAGEPANNEAUCONFIG

Chemin : Configuration utilisateur, Stratégies, Modèles d'administration, Panneau de configuration.

| Paramètre | Valeur |
|---|---|
| Interdire l'accès au Panneau de configuration et aux paramètres du PC | Activé |

![GPO panneau](Ressources/03-gpo-panneau.png)

Capture déjà fournie.

## GPO 4. GPO_COMPUTERS_ADMINLOCAL

Chemin : Configuration ordinateur, Préférences, Paramètres du Panneau de configuration, Utilisateurs et groupes locaux. Je mets à jour le groupe Administrateurs intégré afin d'y ajouter GG_Admins_Locaux. Les membres de ce groupe deviennent ainsi administrateurs locaux des postes.

![GPO admin local](Ressources/04-gpo-adminlocal.png)

Capture fournie présentant la console GPMC et la fenêtre Utilisateurs et groupes locaux. Pour un rendu optimal, une capture en plein écran de la fenêtre Administrateurs intégré faisant apparaître GG_Admins_Locaux serait préférable.

## GPO 5. GPO_COMPUTERS_POWERSHELLSECURITY

Chemin : Configuration ordinateur, Stratégies, Modèles d'administration, Composants Windows, Windows PowerShell.

| Paramètre | Valeur |
|---|---|
| Activer l'exécution des scripts | Activé, scripts locaux et signés distants |
| Journalisation des modules | Activé |
| Journalisation des blocs de scripts | Activé |
| Transcription PowerShell | Activé |

![GPO PowerShell](Ressources/05-gpo-powershell.png)

Capture déjà fournie.

## GPO 6. GPO_COMPUTERS_RENOMMERADMIN

Chemin : Configuration ordinateur, Stratégies, Paramètres Windows, Paramètres de sécurité, Stratégies locales, Options de sécurité.

| Paramètre | Valeur |
|---|---|
| Renommer le compte administrateur | Eco_SuperviseurRoot |
| Renommer le compte Invité | Eco_InviteOff |
| Statut du compte Invité | Désactivé |

![GPO renommer](Ressources/06-gpo-renommer.png)

Capture déjà fournie.

## GPO 7. GPO_COMPUTERS_BLOCAGEUSBSTORAGE

Chemin : Configuration ordinateur, Stratégies, Modèles d'administration, Système, Accès au stockage amovible.

| Paramètre | Valeur |
|---|---|
| Disques amovibles, refuser l'accès en lecture | Activé |
| Disques amovibles, refuser l'accès en écriture | Activé |

![GPO USB](Ressources/07-gpo-usb.png)

Capture déjà fournie.

## Vérification

```
gpupdate /force
gpresult /r
```

CAPTURE A INSERER : la sortie de gpresult /r faisant apparaître les GPO appliquées
Fichier : Ressources/08-gpresult.png
