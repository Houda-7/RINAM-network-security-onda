# ✈️ RINAM : Air Navigation Network Security

Architecture réseau sécurisée conçue pour isoler et protéger les flux critiques aéroportuaires (Télémétrie Radar, VCS) contre les intrusions.

## Stack Technique
* **Infrastructure :** Cisco ISR 4000, Catalyst 2960
* **Réseau :** 802.1Q (VLANs), Inter-VLAN Routing (Router-on-a-stick)
* **Sécurité :** ACLs étendues (Stateless Firewalling), SSHv2 (RSA 1024-bit), IOS Hardening
* **Outil de simulation :** Cisco Packet Tracer

---

## Topologie & Segmentation
Le réseau est divisé en 3 zones isolées (approche Zero-Trust) :
* **VLAN 10 [CRITICAL] :** `192.168.10.0/24` (Systèmes Radar).
* **VLAN 20 [MANAGEMENT] :** `192.168.20.0/24` (Administration & Syslog).
* **WAN [UNTRUSTED] :** `10.0.0.0/8` (Simulation d'attaques externes).

![Topologie de l'architecture](interface-Cisco%20Packet%20Tracer.png)

Isolation niveau 2 et routage interne sur le routeur de bordure :

![Configuration VLAN](vlan%20brief-Switch0.png)

![Interfaces Routeur](Router%20brief-Router0.png)

---

## Security Features & Proof of Concept

### 1. Perimeter Firewalling (ACLs)
Mise en place d'une règle "Default Deny" depuis la zone Untrusted vers le sous-réseau critique (VLAN 10). 
* Les tentatives d'accès (ex: ICMP) sont strictement bloquées par l'interface externe du routeur :
* 
![Ping bloqué par ACL](ACL-PC-Attaquant.png)

* Vérification des paquets rejetés (matches) directement dans la table de l'ACL :
* 
![Matches ACL sur le routeur](access%20lists-Router0.png)

### 2. Device Hardening (OOB Management)
Désactivation des protocoles en clair (Telnet) et obligation d'utiliser un tunnel chiffré SSH pour accéder aux équipements réseau.

![Connexion SSH réussie](SSH-PC-Admin.png)
