# IDS-aide-
🛡️ HIDS & Incident Response Lab : AIDE sur RHEL 10
Déploiement d'un système de détection d'intrusion (HIDS), simulation d'attaques avancées (APT) et investigation numérique (DFIR) sur Red Hat Enterprise Linux 10.
## 📋 Contexte du Projet

Ce dépôt contient les scripts, configurations et le rapport d'investigation d'un laboratoire complet de réponse à incident. L'objectif principal est d'éprouver les mécanismes de **File Integrity Monitoring (FIM)** en utilisant l'outil **AIDE** (Advanced Intrusion Detection Environment) pour détecter des compromissions de bas niveau, des backdoors et des mécanismes de persistance furtifs.

Ce projet a été réalisé dans le cadre du **Master en Cryptographie et Sécurité de l'Information** (Université Mohammed V - Rabat).

## 🛠️ Environnement et Technologies

* **OS :** Red Hat Enterprise Linux (RHEL) 10
* **Outil de détection (HIDS) :** AIDE v0.18.6
* **Analyse de logs :** `journalctl`, `grep`, `sed`
* **Cryptographie :** Hachages SHA-256 et SHA-512 pour la vérification d'intégrité
* **Documentation :** LaTeX

## ⚔️ Scénarios d'Attaque (Red Team)

Pour évaluer la robustesse du système de détection, le serveur a été volontairement compromis en simulant une élévation de privilèges réussie :

1. **Attaques Basiques :**
   * Injection d'un utilisateur root (`UID 0`) caché dans `/etc/passwd`.
   * Altération de la configuration SSH (`PermitRootLogin yes`).
   * Déploiement d'un binaire camouflé (`/usr/local/bin/maintenance`).
   * Empoisonnement DNS local via `/etc/hosts`.
   
2. **Attaques Avancées (Persistance / APT) :**
   * **Cron :** Création d'une tâche planifiée malveillante dans `/etc/crontab`.
   * **Sudoers :** Élévation de privilèges furtive via une règle `NOPASSWD` dans `/etc/sudoers.d/`.
   * **Systemd :** Implantation d'un faux service de démarrage (`sysupdate.service`) pour garantir la survie de la backdoor après redémarrage.

## 🛡️ Détection & Remédiation (Blue Team)

* **Détection (AIDE) :** Identification précise des divergences d'Inodes, d'horodatages (Mtime/Ctime) et des signatures cryptographiques des fichiers altérés.
* **Investigation (DFIR) :** Traque de l'attaquant et reconstruction chronologique de l'incident via l'analyse du démon `systemd-journald`.
* **Remédiation :** Éradication complète des charges utiles, nettoyage des configurations (`sed`, `rm`) et génération d'une nouvelle base de référence intègre.

## 📂 Structure du Dépôt

```text
├── Rapport_Incident_AIDE.pdf      # Rapport complet d'investigation (rédigé en LaTeX)
├── captures_ecran/                # Preuves visuelles des détections et des logs
│   ├── 01_initialisation.png
│   ├── 02_detection_alertes.png
│   └── 03_investigation_logs.png
└── scripts/                       # (Optionnel) Scripts de simulation et de nettoyage
    ├── simulate_attack.sh
    └── remediate_system.sh
