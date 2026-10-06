# REIO — Framework de Co-Design Hardware/Software pour la Sûreté des Systèmes Embarqués

REIO (Réalisme Expérimental Instrumenté Optimisé) est un projet de recherche technologique indépendant (maturité TRL 4) adossé à un dépôt académique Zenodo, proposant des IP Cores pour durcir les architectures embarquées critiques.

## Intérêt Technologique et Innovations

Le framework REIO introduit une rupture méthodologique autour de trois axes principaux :
1. **Alternative Asymétrique aux Multi-Cœurs en Lockstep :** Déporte la détection des fautes sur des sentinelles matérielles (< 50 Slice LUTs) et la reprise en ligne sur un superviseur en Rust bare-metal, pour une consommation de 1 à 3 mW.
2. **Émulation de Logique Trivalente :** Évalue de manière déterministe un troisième état d'incertitude pour confiner localement les incohérences logiques (Single Event Upset).
3. **Isolation Hybride :** Combine un Bus Guardian en logique combinatoire (latence 0 cycle) et des bascules de synchronisation en 1 cycle pour contrer les glitchs et attaques.

## Spécifications et Intérêts Industriels de la Suite d'IP Cores

### REIO-CPU : Séquenceur Durci à Algèbre Ternaire
* **Spécification :** Unité d'exécution et séquenceur SEooC émulant l'algèbre ternaire (20 Slice LUTs).
* **Intérêt Industriel :** Élimine le besoin de doubler intégralement le processeur (Lockstep matériel lourd). Il offre une immunité native contre les dérives de séquencement provoquées par des perturbations radiatives (MBU/SEU), réduisant drastiquement l'empreinte silicium et les coûts de licence IP.
* **Cas d'Utilisation :** Utilisé comme micro-contrôleur de sécurité (Safety Manager) pour superviser l'état des machines à états (FSM) critiques du processeur hôte.

### REIO-DRIVE : Interface d'Acquisition Sécurisée pour Actionneurs
* **Spécification :** Interface d'acquisition durcie pour actionneurs ADAS (21 Slice LUTs).
* **Intérêt Industriel :** Garantit l'intégrité des signaux de commande provenant des capteurs environnementaux (caméras, LIDAR, RADAR). Il empêche l'injection d'ordres aberrants ou de pannes latentes au niveau de la couche physique du véhicule.
* **Cas d'Utilisation :** Placé directement en frontal des contrôleurs de moteurs de direction assistée ou des modules de freinage d'urgence autonome (AEB) pour valider la cohérence des trames de commande.

### REIO-SAFE : Sentinelle de Protection Mémoire (MMU Ultra-Light)
* **Spécification :** Sentinelle d'accès mémoire pour la protection des registres (7 Slice LUTs).
* **Intérêt Industriel :** Apporte une isolation matérielle stricte à un coût de surface dérisoire (7 LUTs). Il empêche les attaques par débordement de tampon (buffer overflow) ou les pointeurs fous d'écraser les registres de configuration matérielle critiques du système.
* **Cas d'Utilisation :** Verrouille dynamiquement l'accès aux registres de configuration des horloges (Clock Gating) et de la gestion de l'alimentation après la phase de boot sécurisé.

### REIO-XBAR : Matrice d'Interconnexion et Confinement (Bus Guardian)
* **Spécification :** Matrice d'Interconnexion Sécurisée et Cellule de Confinement de Bus (Bus Guardian).
* **Intérêt Industriel :** Assure le cloisonnement des fautes (Fault Containment) en temps réel avec une latence nulle. Si un composant non critique devient fou (babbling idiot) et s'accapare le bus, REIO-XBAR l'isole instantanément pour maintenir la disponibilité du reste du système.
* **Cas d'Utilisation :** Positionné comme nœud central de communication entre le cœur de calcul applicatif (non sûr) et les périphériques certifiés ASIL-D.

## Analyse Comparative

| Métrique Critique | Approches Standards (ARM / RISC-V Lockstep) | Suite d'IP Cores REIO |
| :--- | :--- | :--- |
| **Paradigme Logique** | Logique Booléenne Classique (0 / 1) | Émulation de logique trivalente par encodage matériel déterministe |
| **Temps de Réponse aux Fautes** | Plusieurs microsecondes | Déterministe : Confinement matériel en 1 cycle (10 ns) |
| **Consommation Dynamique** | ~500 mW à plusieurs Watts | ~1 mW à 3 mW |
| **Surface d'Empreinte Silicium** | Redondance lourde (Duplication intégrale) | Ultra-compact : < 50 Slice LUTs au total |
| **Cible Réglementaire Visée** | Certifications constructeurs | Conçu pour s'aligner sur ASIL-D (ISO 26262) |

## Statut et Références

Le projet est en validation de concepts (TRL 4). L'accès complet aux netlists, simulations et dossiers de conformité est réservé aux partenaires sous NDA.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20743411.svg)](https://doi.org/10.5281/zenodo.20743411)

## Licence et Propriété Intellectuelle

Ce framework est distribué sous un régime d'**Avis de Propriété Exclusive et Conditions d'Évaluation Publique**. Tous droits réservés © 2026. L'accès public à ce dépôt sert exclusivement de démonstration fonctionnelle et de portfolio de compétences.

Pour consulter l'intégralité des restrictions de reproduction et des conditions d'audit sous NDA, veuillez vous référer au fichier [LICENSE.md](LICENSE.md).

