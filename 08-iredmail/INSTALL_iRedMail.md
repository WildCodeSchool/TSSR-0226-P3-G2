# INSTALL - iRedMail

## 1. Prérequis

Serveur utilisé :

| Élément | Valeur |
|---|---|
| Serveur | SRVLX01 |
| IP | 10.0.10.20 |
| DNS | 10.0.10.10 |
| Passerelle | 10.0.10.254 |
| Domaine | ecotechsolutions.lan |
| FQDN | srvlx01.ecotechsolutions.lan |

Vérifier le FQDN :

```bash
hostname -f
```

Résultat attendu :

```text
srvlx01.ecotechsolutions.lan
```

Si nécessaire, corriger `/etc/hosts` afin d’obtenir un FQDN valide.

## 2. Configuration DNS

Dans la zone DNS `ecotechsolutions.lan`, créer les enregistrements suivants :

```text
A     mail     10.0.10.20
MX    ecotechsolutions.lan -> mail.ecotechsolutions.lan
```

Tests :

```powershell
nslookup mail.ecotechsolutions.lan
nslookup -type=mx ecotechsolutions.lan
```

## 3. Téléchargement et lancement d’iRedMail

Sur `SRVLX01` :

```bash
cd /tmp
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.8.2.tar.gz
tar xvf 1.8.2.tar.gz
cd iRedMail-1.8.2
sudo bash iRedMail.sh
```

## 4. Choix dans l’assistant iRedMail

Sélectionner les paramètres suivants :

| Paramètre | Choix |
|---|---|
| Storage path | /var/vmail |
| Web server | Nginx |
| Backend | MariaDB |
| First mail domain name | ecotechsolutions.lan |
| Mail domain admin | postmaster@ecotechsolutions.lan |
| Optional components | Roundcube, iRedAdmin, Fail2ban |

Ne pas renseigner une adresse mail complète dans le champ `First mail domain name`. Il faut saisir seulement :

```text
ecotechsolutions.lan
```

## 5. Déplacement de GLPI sur le port 8080

GLPI utilisait initialement Apache sur le port `80`. Après installation d’iRedMail, Nginx utilise les ports `80` et `443`.

Modifier le port d’écoute Apache :

```bash
sudo nano /etc/apache2/ports.conf
```

Remplacer :

```text
Listen 80
```

par :

```text
Listen 8080
```

Modifier le VirtualHost GLPI :

```bash
sudo nano /etc/apache2/sites-enabled/glpi.conf
```

Remplacer :

```text
<VirtualHost *:80>
```

par :

```text
<VirtualHost *:8080>
```

Tester la configuration :

```bash
sudo apache2ctl configtest
```

Redémarrer les services :

```bash
sudo systemctl restart apache2
sudo systemctl restart nginx
```

## 6. Correction nftables pour autoriser GLPI

Vérifier les règles nftables :

```bash
sudo nft list ruleset
```

Ajouter la ligne suivante dans `/etc/nftables.conf`, avant la règle `drop` :

```text
tcp dport 8080 accept
```

Tester et recharger la configuration :

```bash
sudo nft -c -f /etc/nftables.conf
sudo systemctl restart nftables
```

Vérifier que la règle est présente :

```bash
sudo nft list ruleset | grep 8080
```

## 7. Vérification des ports

```bash
sudo ss -tlnp | grep -E ':80|:443|:8080|:25|:587|:143|:993'
```

Résultat attendu :

```text
80    nginx
443   nginx
8080  apache2
25    postfix
587   postfix
143   dovecot
993   dovecot
```

## 8. URLs finales

```text
Roundcube : https://10.0.10.20/mail/
iRedAdmin : https://10.0.10.20/iredadmin/
GLPI      : http://10.0.10.20:8080/
```
