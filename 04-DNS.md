# Objectif 4. Service DNS

## Contexte

Le service est hébergé sur SRVWIN01. Le rôle DNS s'installe lors de la promotion du contrôleur de domaine. Le serveur héberge la zone du domaine, une zone de recherche inversée, les enregistrements des machines de l'infrastructure ainsi qu'un redirecteur assurant la résolution externe.

## Zone directe et enregistrements

La zone directe est ecotechsolutions.lan. J'y ai créé un enregistrement de type A pour chaque machine de l'infrastructure.
<img width="695" height="464" alt="image" src="https://github.com/user-attachments/assets/453138d6-862b-400e-b299-143f270275d6" />




## Zone de recherche inversée

J'ai configuré une zone de recherche inversée pour le réseau 10.0.10.0/24, accompagnée de ses enregistrements PTR.
<img width="660" height="321" alt="image" src="https://github.com/user-attachments/assets/54fb4c91-01a8-45fb-8e9b-96943c63ebe4" />

## Redirecteur et résolution externe

J'ai configuré un redirecteur pointant vers 10.0.10.254, à savoir le pfSense. Lorsqu'une machine sollicite un nom externe que le serveur ne connaît pas, la requête est transmise au pfSense, qui la relaie à son tour vers le FAI. La résolution interne et la résolution externe constituent ainsi deux mécanismes distincts, gérés par le même serveur.

RAPPEL METTRE CAPTURE -> l'onglet Redirecteurs faisant apparaître 10.0.10.254


RAPPEL METTRE LAUTRE CAPTURE > un nslookup google.com aboutissant, démontrant la résolution externe de bout en bout

