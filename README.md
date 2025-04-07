# 📡 Protocoles TCP vs UDP – Ports, Usages & Explications

Comparaison complète des protocoles réseau par type de transport (TCP, UDP ou autre). Ce document explique les **ports d'écoute standard**, les **raisons techniques** derrière le choix du protocole, et donne une vision claire pour les sysadmins, étudiants ou auditeurs réseau.

---

## 🧭 Sommaire

- [🔁 Différences TCP vs UDP](#-différences-tcp-vs-udp)
- [🔷 Protocoles TCP uniquement](#-protocoles-tcp-uniquement)
- [🔶 Protocoles UDP uniquement](#-protocoles-udp-uniquement)
- [🟣 Protocoles TCP et UDP](#-protocoles-tcp-et-udp)
- [⚙️ Protocoles sans TCP ni UDP](#️-protocoles-sans-tcp-ni-udp)
- [🧠 Pourquoi certains protocoles utilisent UDP ?](#-pourquoi-certains-protocoles-utilisent-udp-)
- [🔐 Pourquoi d’autres nécessitent TCP ?](#-pourquoi-dautres-nécessitent-tcp-)

---

## 🔁 Différences TCP vs UDP

| Transport | Avantage principal         | Caractéristiques |
|----------|-----------------------------|------------------|
| **TCP**  | Fiabilité & Ordre           | Connexion, accusés de réception, retransmissions |
| **UDP**  | Vitesse & Légèreté          | Pas de connexion, pas de garantie, moindre overhead |

---

## 🔷 Protocoles TCP uniquement

| Protocole | Port(s) | Usage principal | Pourquoi TCP ? |
|----------|--------|------------------|----------------|
| **HTTP**       | `80`           | Web non sécurisé       | Connexion persistante, transmission fiable |
| **HTTPS**      | `443`          | Web sécurisé           | Chiffrement SSL/TLS nécessite TCP |
| **SSH**        | `22`           | Accès distant sécurisé | Authentification, fiabilité obligatoire |
| **FTP (commande)** | `21`       | Contrôle de fichiers   | Échange structuré de commandes |
| **FTPS (implicite)** | `990`     | FTP sécurisé           | Encapsulation TLS nécessite TCP |
| **SMTP**       | `25`, `587`, `465` | Envoi d’emails      | Commandes texte fiables, TLS |
| **IMAP**       | `143`, `993`   | Lecture email          | États et dossiers synchronisés |
| **POP3**       | `110`, `995`   | Lecture email          | Téléchargement séquentiel fiable |
| **RDP**        | `3389`         | Bureau distant Windows | Connexion graphique persistante |
| **Telnet**     | `23`           | Terminal non chiffré   | Flux de commande interactif |
| **TACACS+**    | `49`           | Auth admin Cisco       | Suivi précis des commandes, logs |
| **BGP**        | `179`          | Routage Internet       | Session longue, sensible à la perte |

---

## 🔶 Protocoles UDP uniquement

| Protocole | Port(s) | Usage principal | Pourquoi UDP ? |
|----------|--------|------------------|----------------|
| **DNS**        | `53`           | Résolution de nom       | Réponses rapides, petit volume |
| **DHCP**       | `67` / `68`    | Attribution IP          | Diffusion LAN, rapide |
| **SNMP**       | `161`, `162`   | Monitoring              | Petits paquets, polling ou traps |
| **TFTP**       | `69`           | Transfert fichier simple| Pas d’état, très léger |
| **Syslog**     | `514`          | Logs centralisés        | Envoi non critique |
| **NetFlow**    | `2055`         | Statistiques trafic     | Export rapide de flux |
| **sFlow**      | `6343`         | Échantillonnage trafic  | Pas de fiabilité requise |
| **RIP**        | `520`          | Routage LAN             | Routage simple, tolère perte |
| **SSDP**       | `1900`         | Découverte UPnP         | Multicast, no ACK |
| **mDNS**       | `5353`         | DNS local (.local)      | Multicast sans retour |
| **LLMNR**      | `5355`         | Résolution nom local Win| Remplaçant NetBIOS |
| **NTP**        | `123`          | Synchronisation heure   | Réponses rapides, broadcast |

---

## 🟣 Protocoles TCP et UDP

| Protocole | TCP Port(s) | UDP Port(s) | Raison du double usage |
|----------|-------------|-------------|-------------------------|
| **DNS**        | `53`          | `53`          | UDP pour requêtes simples, TCP pour zones/longues réponses |
| **LDAP**       | `389`         | `389`         | TCP pour AD, UDP possible pour découverte |
| **SIP**        | `5060`        | `5060`        | UDP fréquent (VoIP), TCP/TLS pour fiabilité ou sécurité |
| **FTPS (explicite)** | `21`     | –             | TCP utilisé, UDP non supporté |
| **RADIUS**     | –             | `1812`, `1813`| Utilise UDP, mais TLS sur TCP dans RadSec |
| **IKE/IPsec**  | –             | `500`, `4500` | UDP pour la négociation initiale, IPsec via protocoles IP bruts |

---

## ⚙️ Protocoles sans TCP ni UDP

| Protocole | IP Protocol Number | Utilisation | Pourquoi pas TCP/UDP ? |
|----------|---------------------|-------------|-------------------------|
| **ICMP**      | `1`   | Ping, traceroute | Contrôle direct, sans session |
| **IGMP**      | `2`   | Multicast         | Gestion de groupes multicast IP |
| **GRE**       | `47`  | Tunneling         | Transport d’un autre protocole (ex: IPv6) |
| **ESP (IPsec)** | `50` | Chiffrement VPN   | Chiffre les paquets IP eux-mêmes |
| **AH (IPsec)** | `51` | Authentification  | Ajoute intégrité/authentification à IP |
| **OSPF**      | `89`  | Routage interne   | Envoi direct de LSA, pas de session |
| **EIGRP**     | `88`  | Routage Cisco     | Échange de tables, sans transport TCP |
| **IS-IS**     | CLNS  | Routage opérateur | Non basé sur IP, mais sur liaison (L2) |

---

## 🧠 Pourquoi certains protocoles utilisent UDP ?

- Moins de surcharge réseau : pas d'établissement de connexion ni d'accusé de réception
- Plus rapide et réactif, idéal pour :
  - Requêtes simples (DNS)
  - Logs système (Syslog)
  - Monitoring réseau (SNMP)
  - Communication en temps réel (VoIP, RTP)
- La perte de paquets est tolérable dans le contexte

---

## 🔐 Pourquoi d’autres nécessitent TCP ?

- Garantie de livraison, ordre, et contrôle de flux
- Prérequis pour des connexions durables, authentifiées, ou chiffrées :
  - SSH, HTTPS, SMTP, RDP…
- Permet le dialogue interactif (ex: Telnet, FTP)
- Utilisé là où la perte de paquets est critique

---

