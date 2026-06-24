# USER GUIDE - iRedMail

## 1. Accès au webmail

Le webmail Roundcube est accessible depuis un navigateur :

```text
https://10.0.10.20/mail/
```

ou via le nom DNS :

```text
https://mail.ecotechsolutions.lan/mail/
```

Une alerte de certificat peut apparaître. Dans le cadre du laboratoire, le certificat utilisé est auto-signé.

## 2. Connexion utilisateur

L’utilisateur doit saisir son adresse mail complète.

Exemple :

```text
lucas.bernard@ecotechsolutions.lan
```

## 3. Envoyer un mail

Depuis Roundcube :

1. Cliquer sur **Créer un message**.
2. Renseigner le destinataire.
3. Renseigner l’objet.
4. Rédiger le message.
5. Cliquer sur **Envoyer**.

Exemple de test :

```text
Expéditeur   : lucas.bernard@ecotechsolutions.lan
Destinataire : lea.petit@ecotechsolutions.lan
Objet        : Bonjour Lea !
```

## 4. Vérifier la réception

Se connecter avec le compte destinataire :

```text
lea.petit@ecotechsolutions.lan
```

Le mail doit apparaître dans la boîte de réception.

## 5. Administration des comptes

L’administration des comptes mail se fait via iRedAdmin :

```text
https://10.0.10.20/iredadmin/
```

L’administrateur peut :

- créer des boîtes mail ;
- modifier les mots de passe ;
- désactiver un compte ;
- gérer les comptes du domaine `ecotechsolutions.lan`.

## 6. Accès à GLPI après installation d’iRedMail

GLPI reste disponible sur le même serveur, mais sur le port `8080` :

```text
http://10.0.10.20:8080/
```

Le webmail et l’administration iRedMail utilisent Nginx sur les ports `80` et `443`, tandis que GLPI utilise Apache sur le port `8080`.

## 7. Points d’attention

Ne jamais publier de mot de passe dans la documentation GitHub.

Avant d’ajouter une capture d’écran, vérifier qu’elle ne contient pas :

- mot de passe ;
- token ;
- information sensible inutile ;
- donnée privée.
