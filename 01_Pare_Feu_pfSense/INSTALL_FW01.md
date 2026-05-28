# Installation et Configuration — Pare-feu pfSense (FW01)

## Sommaire

1. [Prérequis techniques](#1-prérequis-techniques)
2. [Création de la VM](#2-création-de-la-vm)
3. [Installation de pfSense](#3-installation-de-pfsense)
4. [Configuration des interfaces réseau](#4-configuration-des-interfaces-réseau)
5. [Configuration des règles de pare-feu](#5-configuration-des-règles-de-pare-feu)
6. [FAQ / Problèmes connus](#6-faq--problèmes-connus)

---

## 1. Prérequis techniques

| Élément | Valeur |
|---|---|
| ISO | pfSense CE (dernière version stable) |
| RAM | 1 Go minimum |
| Disque | 10 Go |
| Interfaces réseau | 3 (WAN, LAN, DMZ) |
| Hyperviseur | VirtualBox |

## 2. Création de la VM

> *(À compléter avec captures d'écran)*

## 3. Installation de pfSense

> *(À compléter avec captures d'écran)*

## 4. Configuration des interfaces réseau

| Interface | Zone | Réseau | IP |
|---|---|---|---|
| em0 | WAN | DHCP (box FAI) | Automatique |
| em1 | LAN | `10.0.10.0/24` | `10.0.10.254` |
| em2 | DMZ | `10.0.20.0/24` | `10.0.20.254` |

> *(À compléter avec captures d'écran)*

## 5. Configuration des règles de pare-feu

Principe : **Deny All** — tout est bloqué par défaut, on ouvre uniquement ce qui est nécessaire.

> *(À compléter avec les règles WAN, LAN, DMZ)*

## 6. FAQ / Problèmes connus

> *(À compléter)*
