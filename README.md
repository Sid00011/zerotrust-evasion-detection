# Zero-Trust Evasion Detection

Research study on the structural limits of Zero-Trust architectures as deployed today,
and how a sophisticated attacker with an initial foothold can bypass detection while
moving laterally through the system.

[Read the full report (PDF)](https://github.com/Sid00011/zerotrust-evasion-detection/blob/main/Sid_Rapport_Recherche_ZeroTrust.pdf)

---

## Research question

What are the structural limits of Zero-Trust architectures as currently deployed,
and how can an attacker with an initial entry point exploit those limits to evade
detection while progressing through the information system?

---

## Summary

The Zero-Trust model rests on three foundational assumptions that are rarely made
explicit - identity provider (IdP) integrity, telemetry completeness, and
micro-segmentation granularity. Sophisticated attackers exploit these assumptions
systematically.

This study analyzes four structural evasion vectors, maps them to MITRE ATT&CK,
confronts them against documented public incidents (Okta 2023, Scattered Spider),
and proposes an original 8-dimension maturity evaluation grid.

---

## Four evasion vectors analyzed

| Vector | Assumption targeted | MITRE technique | Typical detection |
|---|---|---|---|
| Identity provider (IdP) abuse | H1 - IdP integrity | T1556 / .007 / .009 | IdP logs, ITDR |
| Telemetry blind spots | H2 - Complete telemetry | T1071.004 / T1041 | NDR, UEBA, beaconing |
| Micro-segmentation gaps | H3 - Effective granularity | T1021 / T1210 / T1570 | UEBA, attack graph |
| Session token hijacking | H1 + H2 | T1539 / T1550.004 | Impossible travel, ASN |

---

## Experimental lab

**Environment:** VMware Workstation 17 + GNS3, isolated network 192.168.56.0/24

| Component | Role |
|---|---|
| Keycloak 24 | Identity provider (OIDC + MFA TOTP) |
| Flask + NGINX + oauth2-proxy | Policy enforcement point (PEP) |
| ELK Stack 8 | SIEM (Elasticsearch, Logstash, Kibana) |
| Kali Linux | Attacker machine (Mitmproxy, Evilginx2, Python scripts) |
| Windows 11 | Victim machine (Chromium, enrolled MFA) |

**Main finding:** Session token replay (pass-the-cookie) produces zero authentication
anomalies in a default Zero-Trust configuration. The attack is undetectable without
explicit correlation rules targeting source IP, User-Agent, and access frequency.
This holds even when MFA is enforced - once the session cookie is stolen post-authentication,
MFA provides no protection.

---

## Original contribution - Zero-Trust maturity grid

An 8-dimension self-assessment tool focused on evasion resistance. Complements the
CISA Zero Trust Maturity Model v2.0 and is directly applicable to ANSSI accreditation
(BP-028) and NIS2 compliance audits (Article 21).

| # | Dimension | Level 0 | Level 3 (target) |
|---|---|---|---|
| 1 | Phishing-resistant authentication | Password + SMS | FIDO2 / universal passkeys |
| 2 | Token-device binding | Standard cookie | DPoP or client mTLS |
| 3 | Token lifetime | >= 8 hours | <= 15 min + rotation |
| 4 | Telemetry coverage | Perimeter logs only | SIEM + NDR + UEBA + EDR correlated |
| 5 | C2 behavioral detection | None | ML beaconing models + threat hunting |
| 6 | Micro-segmentation | Single VLAN | L7 policy per workload |
| 7 | Device posture | Not verified | Continuous control (MDM / EDR) |
| 8 | ITDR and IdP governance | None | Active ITDR + quarterly review |

**Score interpretation:**

- Below 12/24 - architecture bypassable on at least one vector
- 12 to 18/24 - intermediate level, detection possible but manual
- 19 to 24/24 - advanced maturity, automated and tested controls

---

## Key references

- NIST SP 800-207 - Zero Trust Architecture (2020)
- ANSSI-PA-111 - The Zero Trust Model: Fundamentals (June 2025)
- CISA Zero Trust Maturity Model v2.0 (2023)
- MITRE ATT&CK for Enterprise
- Verizon DBIR 2025
- Okta Security - Incident Report (November 2023)
- Basta et al. - arXiv:2111.10967
- Xavier et al. - arXiv:2209.00943

---

## Keywords

zero-trust · evasion detection · MITRE ATT&CK · session hijacking ·
micro-segmentation · identity provider · SIEM · ANSSI · NIS2 · pass-the-cookie

---

*Author: Sidali - Cybersecurity Research - May 2026*
