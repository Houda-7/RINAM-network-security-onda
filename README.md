# 🛡️ Sécurisation de l'Infrastructure Réseau Critique (RINAM - ONDA)

## 📌 Contexte du Projet
Ce projet est une maquette de validation technique issue de mon stage d'initiation au sein du **Service Technique Navigation de l'Aéroport Chérif El Idrissi (ONDA)**. En tant qu'étudiante en Réseaux et Télécommunications à l'EST Nador, j'ai conçu ce laboratoire pour simuler et sécuriser le **Réseau IP de la Navigation Aérienne Marocaine (RINAM)** face aux menaces internes et externes.

L'objectif principal est de garantir la haute disponibilité, l'intégrité et la confidentialité des flux critiques (Données Radar, Serveurs VCS) en appliquant des politiques de sécurité strictes sur des équipements Cisco (Routeur ISR 4331 series et Commutateur Catalyst 2960).

## 🏗️ Architecture et Segmentation (VLANs)
Pour limiter la surface d'attaque, le réseau a été segmenté logiquement :
* **VLAN 10 (RADAR & Flight Data) :** `192.168.10.0/24` - Zone ultra-critique hébergeant le trafic de la navigation aérienne.
* **VLAN 20 (ADMIN & SYSLOG) :** `192.168.20.0/24` - Zone de supervision et d'administration.
* **Zone Externe (WAN / Attaquant) :** `10.0.0.0/8` - Réseau non sécurisé simulant une tentative d'intrusion.

### Topologie du Réseau
![Topologie de l'architecture](interface-Cisco%20Packet%20Tracer.png)

### Configuration des VLANs (Isolation L2)
Configuration du Switch Core avec affectation stricte des ports d'accès et montage d'un lien Trunk (802.1Q) vers le routeur (Router-on-a-Stick).
![Configuration VLAN](vlan%20brief-Switch0.png)

### Routage Inter-VLAN (Sub-interfaces)
![Interfaces Routeur](Router%20brief-Router0.png)

## 🔒 Solutions de Sécurité Implémentées

### 1. Filtrage et Contrôle d'Accès (ACL Étendues)
Mise en place d'un pare-feu stateless sur le routeur de bordure pour empêcher tout accès non autorisé depuis l'extérieur vers la zone critique, tout en autorisant le trafic légitime.
* **Résultat :** Les tentatives de ping/intrusion depuis la zone externe (`10.0.0.10`) vers le serveur Radar (`192.168.10.10`) sont bloquées par le routeur (`Destination host unreachable`).

![Ping bloqué par ACL](ACL-PC-Attaquant.png)

![Matches ACL sur le routeur](access%20lists-Router0.png)

### 2. Durcissement de l'Administration (SSHv2)
Désactivation des protocoles en clair (Telnet) au profit du protocole chiffré SSH (Clé RSA 1024 bits) pour l'accès distant aux équipements de cœur de réseau.
* **Résultat :** Authentification locale requise, flux de configuration protégés contre le sniffing.

![Connexion SSH réussie](SSH-PC-Admin.png)

## 🛠️ Outils & Technologies
* **Simulation :** Cisco Packet Tracer
* **Réseau :** Routage Inter-VLAN (802.1Q), Commutation L2.
* **Sécurité :** ACL Étendues, SSHv2, Durcissement Cisco IOS.

## 🚀 Perspectives
Ce socle technique est conçu pour évoluer vers l'intégration d'un véritable Next-Generation Firewall (Fortinet FortiGate) et la centralisation des logs vers une solution SIEM pour la détection d'anomalies assistée par l'Intelligence Artificielle.
