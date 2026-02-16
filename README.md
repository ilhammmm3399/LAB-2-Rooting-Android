# LAB-2-Rooting-Android
Auteur : Ilham Bazar | Date : 15 Février 2026.

1. Introduction :

Le rooting, c'est avoir les privilèges super-utilisateur (root) sur le système Android. Ceci permet l'accès à des zones normalement verrouillées par le fabricant. En laboratoire, le rooting sert à observer les comportements internes des applications et du noyau.

2. Fiche Périmètre 

| Élément | Détails du projet |
| --- | --- |
| **Application** | DIVA (Dammit Vulnerable Android App) Version Beta |
| **Support** | Émulateur Android Studio (AVD) Pixel 6 |
| **OS / API** | Android 16.0 / API 36 (Hôte Fedora) |
| **Objectif** | Comprendre le rooting et ses impacts sur la sécurité |
| **Données** | Utilisation exclusive de données fictives |

✅ Checklist de Début :

- Périmètre écrit

- AVD neuf

<img width="1398" height="942" alt="image" src="https://github.com/user-attachments/assets/f75946bf-a3fa-43b2-ada6-136fd91c6005" />

- App test installée

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/f8876655-e209-45c1-880a-50a9ea515110" />

- Scenario (taper sur une option)
<img width="708" height="1496" alt="image" src="https://github.com/user-attachments/assets/184cb1a1-77cf-4cc5-b184-bc95b9706785" />
<img width="708" height="1496" alt="Screenshot From 2026-02-14 12-50-24" src="https://github.com/user-attachments/assets/2ce611e6-e2b5-4013-91a3-1e3b04c8ab3d" />



- Versions Android/app notées 

3. Architecture de Sécurité :

- Couches de Sécurité :

*Sandboxing* : Chaque application est isolée dans son propre environnement protege.

*Permissions* : Contrôle d'accès strict aux ressources systeme.

*Intégrité Système* : proteger le systeme des modifications non autorisee.

- Schéma : Chaîne de Confiance (Verified Boot / AVB)

 "Chain Of Trust" est une série de vérifications où chaque composant vérifie l'authenticité du suivant avant de lui faire confiance.

 >[Matériel] → [Bootloader] → [Kernel/OS] → [Applications]

Le Verified Boot est un système d'alarme qui verifie l'intégrité à chaque démarrage.

L'intégrité au démarrage est critique puisque toutes les protections ultérieures peuvent être contournées si elle n'est pas maintenue.

L'AVB (v2.0) utilise une protection anti-rollback contre l'installation de versions vulnérables.

- Commande de vérification :

>adb shell getprop ro.boot.verifiedbootstate   

<img width="3072" height="544" alt="Screenshot From 2026-02-14 12-56-02" src="https://github.com/user-attachments/assets/fd123c3a-6ba8-4aea-801e-59844c5f6308" />

    Green : Tout est normal, système vérifié et intègre
    Yellow/Orange : Avertissement, système modifié mais fonctionnel
    Red : Danger, intégrité compromise

    
4. Réalisation Technique :

- Synthèse des Commandes de Rooting


| Commande | Action effectuée |
| --- | --- |
| `adb root` | Démarre le serveur ADB avec les droits super-utilisateur. |
| `adb disable-verity` | Désactive la vérification d'intégrité des partitions système. |
| `adb reboot` | Redémarre l'appareil pour appliquer les changements. |
| `adb remount` | Monte les partitions système en mode lecture/écriture. |

<img width="1566" height="172" alt="image" src="https://github.com/user-attachments/assets/cd9ac6dd-cfdb-4c93-9c86-1f9e19116e40" />

<img width="1536" height="1496" alt="image" src="https://github.com/user-attachments/assets/2e48c70d-0df9-4f16-ac12-c7a8aea11db8" />

<img width="1600" height="369" alt="image" src="https://github.com/user-attachments/assets/a3c06cd1-59cb-4fc4-9220-c46e3381b696" />







- En laboratoire, cet environnement privilégié permet d'analyser les comportements à bas niveau et de tester la robustesse du stockage face à un attaquant ayant déjà compromis le système. C'est comme avoir les clés passe-partout du bâtiment pour vérifier que, même si un intrus entre, les coffres-forts sont impossibles à ouvrir.

5. Gestion des Risques & Mesures Défensives 

| Risque Identifié (Étape 11) | Mesure Défensive Appliquée (Étape 12) |
| --- | --- |
| **1. Communication non contrôlée** | **Réseau isolé** pour éviter toute communication vers l'extérieur. |
| **2. Fuite de données réelles** | **Données fictives uniquement** pour éliminer tout risque de fuite. |
| **3. Exposition hors labo** | **Device/AVD dédié** exclusivement aux tests de sécurité. |
| **4. Traces résiduelles** | **Snapshots ou wipe** en fin de séance pour ne laisser aucune trace. |
| **5. Non-reproductibilité** | **Journal de configuration** détaillé pour assurer la reproductibilité. |
| **6. Mélange de données** | **Aucun compte personnel** utilisé pour éviter les fuites privées. |
| **7. Compromission système** | **Contrôle strict des APK** installées pour limiter les risques. |
| **8. Absence de preuves** | **Horodatage + captures** des étapes pour une traçabilité complète. |

6. Standards OWASP

| Standard | Application pratique |
| --- | --- |
| **MASVS STORAGE-1** | Vérifier si les secrets (clés, mots de passe) sont chiffrés au repos. |
| **MASVS NETWORK-1** | S'assurer de l'utilisation sécurisée de TLS et des certificats. |
| **MASTG Test 1** | Explorer `/data/data/` via root pour trouver des secrets en clair. |
| **MASTG Test 2** | Analyser `adb logcat` pour détecter des fuites d'infos sensibles. |

7. Traçabilité & Clôture 

- Preuve extraite dans logcat_root_check.txt.
<img width="1600" height="737" alt="image" src="https://github.com/user-attachments/assets/d82d2d92-4e73-42c1-93ef-7867a2735765" />



- Reset via UI
<img width="740" height="304" alt="image" src="https://github.com/user-attachments/assets/cd2ca1aa-d5b4-4e08-b384-3cd4d739141b" />
<img width="1600" height="737" alt="image" src="https://github.com/user-attachments/assets/b27cb756-b4cb-49f4-8e15-37e6271a8340" />



✅ Checklist de Fin 

    [/] Données supprimées : Nettoyage des fichiers de test.

    [/] Reset effectué : Wipe Data de l'AVD (via UI ou commande).

    [/] Preuve du reset : Assistant initial visible au redémarrage.

    [/] Sauvegarde : Rapport et captures sécurisés.

    [/] Hygiène : Aucun compte personnel n'a été utilisé.


Statut du Laboratoire : Terminé & Réinitialisé (Méthodologie PDCA validée).
