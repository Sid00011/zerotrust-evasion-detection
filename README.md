# Évasion de Détection dans les Environnements Zero-Trust
### Limites structurelles des architectures actuelles

**Auteur :** Sidali  
**Date :** Mai 2026

---

## Question de recherche

Quelles sont les limites structurelles des architectures Zero-Trust
telles qu'elles sont déployées aujourd'hui, et comment un attaquant
disposant d'un point d'entrée initial peut-il exploiter ces limites
pour échapper à la détection tout en progressant dans le système
d'information ?

---

## Résumé

Le modèle Zero-Trust repose sur trois hypothèses constitutives rarement
explicitées — intégrité du fournisseur d'identité (IdP), complétude de
la télémétrie, granularité de la micro-segmentation — que des acteurs
sophistiqués exploitent de manière systématique.

Ce rapport analyse quatre vecteurs d'évasion structurels, les cartographie
dans MITRE ATT&CK, les confronte à des incidents publics documentés
(Okta 2023, Scattered Spider), et propose une grille d'évaluation de
maturité originale en huit dimensions.

---

## Les quatre vecteurs analysés

| Vecteur | Hypothèse | Technique MITRE | Détection typique |
|---|---|---|---|
| Abus du fournisseur d'identité (IdP) | H1 - Intégrité IdP | T1556 / .007 / .009 | Logs IdP, ITDR |
| Angles morts télémétriques | H2 - Télémétrie complète | T1071.004 / T1041 | NDR, UEBA, beaconing |
| Défauts de micro-segmentation | H3 - Granularité effective | T1021 / T1210 / T1570 | UEBA, graphe d'attaque |
| Détournement de jetons de session | H1 + H2 | T1539 / T1550.004 | Déplacement impossible, ASN |

---

## Scénario expérimental

**Environnement :** VMware Workstation 17 + GNS3, réseau isolé 192.168.56.0/24

**Infrastructure déployée :**
- Keycloak 24 - Fournisseur d'identité (OIDC + MFA TOTP)
- Flask + NGINX + oauth2-proxy — Point d'application des politiques (PEP)
- Stack ELK 8 - SIEM (Elasticsearch, Logstash, Kibana)
- Kali Linux - Poste attaquant (Mitmproxy, Evilginx2, scripts Python)
- Windows 11 - Poste victime (Chromium, MFA enrôlé)

**Résultat principal :** Le rejeu de jeton de session (pass-the-cookie)
ne produit aucune anomalie d'authentification dans une configuration
par défaut. L'attaque est indétectable sans règles de corrélation
contextuelle explicites portant sur l'adresse IP source, le User-Agent
et la fréquence d'accès.

---

## Grille de maturité Zero-Trust — contribution originale

Outil d'auto-évaluation en huit dimensions, orienté résistance à
l'évasion. Complémentaire du CISA Zero Trust Maturity Model v2.0,
directement utilisable dans une démarche d'homologation ANSSI (BP-028)
ou un audit de conformité NIS2 (article 21).

| # | Dimension | Niveau 0 | Niveau 3 (cible) |
|---|---|---|---|
| 1 | Authentification résistante au phishing | Mot de passe + SMS | FIDO2 / passkeys universel |
| 2 | Liaison jeton / appareil | Cookie classique | DPoP ou mTLS client |
| 3 | Durée de vie du jeton | ≥ 8 h | ≤ 15 min + rotation |
| 4 | Couverture télémétrique | Logs périmétrique | SIEM + NDR + UEBA + EDR corrélés |
| 5 | Détection comportementale C2 | Aucune | Modèles ML beaconing + threat hunting |
| 6 | Micro-segmentation | VLAN unique | Politique L7 par charge de travail |
| 7 | Posture de l'appareil | Non vérifiée | Contrôle continu (MDM / EDR) |
| 8 | ITDR et gouvernance IdP | Aucune | ITDR actif + revue trimestrielle |

**Interprétation du score :**
- Moins de 12/24 → architecture contournable sur au moins un vecteur
- De 12 à 18/24 → niveau intermédiaire, détection possible mais manuelle
- De 19 à 24/24 → maturité avancée, contrôles automatisés et testés

---

## Références principales

- NIST SP 800-207 - Zero Trust Architecture (2020)
- ANSSI-PA-111 - Le modèle Zero Trust : les fondamentaux (juin 2025)
- CISA Zero Trust Maturity Model v2.0 (2023)
- MITRE ATT&CK for Enterprise
- Verizon DBIR 2025
- Okta Security - Rapport d'incident (novembre 2023)
- Basta et al. - arXiv:2111.10967
- Xavier et al. - arXiv:2209.00943

---

## Mots-clés

`zero-trust` `cybersécurité` `MITRE-ATT&CK` `ANSSI` `évasion-détection`
`session-hijacking` `micro-segmentation` `IdP` `SIEM` `homologation` `NIS2`
