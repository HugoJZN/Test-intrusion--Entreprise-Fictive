# Test d'intrusion par ingénierie sociale — « Entreprise Fictive »

> Projet d'**Ethical Hacking** (ST2EH — EFREI). Compromission d'un poste de travail
> via un document Excel macro-piégé, en environnement de laboratoire isolé.

![Statut](https://img.shields.io/badge/cadre-p%C3%A9dagogique-blue)
![Type](https://img.shields.io/badge/type-proof%20of%20concept-orange)
![Diffusion](https://img.shields.io/badge/diffusion-responsable-green)

---

## ⚠️ Avertissement (à lire en premier)

Ce dépôt documente un exercice **pédagogique** réalisé dans un cadre **autorisé**, sur une
**entreprise fictive** et des personae **inventées**, dans un **laboratoire virtualisé et isolé**
(aucun système réel visé, aucune donnée réelle touchée).

**Diffusion responsable :** ce dépôt contient un **rapport et une analyse**, **pas** d'outil
d'attaque réutilisable. Les éléments réellement « armés » (shellcode, macro complète avec
charge, payloads compilés, corps du courriel de phishing, techniques d'évasion AV) sont
**volontairement omis**. L'objectif est de **comprendre la méthode pour mieux s'en défendre**.

L'utilisation de ces techniques hors d'un cadre autorisé est **illégale** (en France,
articles 323-1 et suivants du Code pénal). Les auteurs déclinent toute responsabilité
en cas d'usage détourné.

---

## 🎯 Contexte & objectif

« Entreprise Fictive » est une société fictive de e-commerce. La mission : simuler une
attaque **réaliste** pour identifier et exploiter ses vulnérabilités. Choix stratégique :
viser le **maillon humain** plutôt que les serveurs, via un scénario de **spear-phishing**
ciblant un collaborateur du service financier.

**Verdict :** compromission complète d'un poste (jusqu'aux **privilèges administrateur**,
persistance et effacement des traces) **sans exploiter aucune faille logicielle** — uniquement
de l'OSINT, un prétexte crédible et des faiblesses de configuration.

## 🧭 Méthodologie

Les **5 phases** de l'ethical hacking, mises en correspondance avec la **Cyber Kill Chain**
et **MITRE ATT&CK**.

| # | Phase | Résumé | Techniques ATT&CK (extrait) |
|---|-------|--------|------------------------------|
| 1 | Reconnaissance (OSINT) | Identification de la cible et de l'identité à usurper | T1589, T1591, T1593 |
| 2 | Scanning & Énumération | Qualification des surfaces (humaine, messagerie, logicielle) | T1589.002 |
| 3 | Gaining Access | Leurre Excel « chiffré » + spear-phishing + exécution | T1566.001, T1204.002, T1059.005 |
| 4 | Maintaining Access | Élévation (bypass UAC), migration, persistance | T1548.002, T1547.001 |
| 5 | Covering Tracks | Falsification d'horodatages, effacement des journaux | T1070.006, T1070.001 |

### Environnement (laboratoire)

| Rôle | Système | Détails |
|------|---------|---------|
| Attaquant | Kali Linux | Metasploit, msfvenom, swaks |
| Cible | Windows 10 x64 | Office (macros), WinRAR |
| Messagerie | PaperCut SMTP | serveur mail de test (installé sur la VM Windows) |

> Les adresses IP et mots de passe du laboratoire ont été masqués ou retirés.

## 🔑 Le cœur du projet — le leurre « document chiffré »

Un classeur RH réaliste (`.xlsm`) présente un **faux bandeau « DOCUMENT CHIFFRÉ »** et une
**feuille volontairement protégée** : toute tentative de modification est bloquée, ce qui
**pousse l'utilisateur à cliquer sur « Activer le contenu »**. Ce clic déclenche une macro qui
(1) **joue l'illusion d'un déchiffrement** (retire la protection, masque le bandeau, affiche un
message de succès) et (2) **exécute en silence** la charge. C'est de l'**ingénierie sociale par
le document lui-même**.

## 🛡️ Synthèse des vulnérabilités

| ID | Vulnérabilité | Criticité |
|----|---------------|-----------|
| V1 | Exposition OSINT (employés, format e-mail) | Moyenne |
| V2 | Messagerie sans anti-usurpation (SPF/DKIM/DMARC) | Élevée |
| V3 | Macros Office exécutables sans restriction | Élevée |
| V4 | Archives chiffrées non filtrées | Moyenne |
| V5 | Absence de sensibilisation des utilisateurs | Élevée |
| V6 | Admin local + UAC contournable | Élevée |
| V7 | Absence d'antivirus / EDR actif | **Critique** |
| V8 | Persistance (Run, Temp) non supervisée | Élevée |
| V9 | Journaux non centralisés (SIEM/WEF) | Élevée |

## 🧩 Recommandations (défense en profondeur)

- **Messagerie :** SPF/DKIM/DMARC en rejet, relais authentifié, filtrage des archives chiffrées.
- **Poste de travail :** blocage des macros Internet, règles ASR, retrait de l'admin local, EDR + anti-sabotage.
- **Détection :** journaux centralisés (SIEM, Sysmon, WEF), alerte sur l'événement 1102, FIM.
- **Facteur humain :** sensibilisation, phishing simulé, vérification hors-canal des demandes urgentes.

## 📂 Contenu du dépôt

```
.
├── README.md
├── LICENSE
├── rapport/            # Rapport complet (PDF) — shellcode caviardé
├── presentation/       # Support de soutenance (PDF)
└── captures/           # Captures d'écran (sans contenu exploitable)
```

## 🧰 Outils utilisés

OSINT (Google Dorks, Skrapp) · Metasploit / msfvenom / Meterpreter · swaks · WinRAR · VBA (macros Office).

## 👥 Auteurs

Enzo Juzyna · Hugo Juzyna · Ismael Radouane — EFREI, module ST2EH (Ethical Hacking).

## 📄 Licence

Voir [`LICENSE`](LICENSE). Contenu fourni à des fins **éducatives** uniquement.
