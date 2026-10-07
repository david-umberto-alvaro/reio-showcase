# REIO — Framework de Co-Design Hardware/Software pour la Sûreté des Systèmes Embarqués

REIO (Réalisme Expérimental Instrumenté Optimisé) est un projet de recherche technologique indépendant (TRL 4) adossé à un dépôt académique Zenodo, proposant une suite d'IP Cores et un processeur de confiance pour durcir les architectures embarquées critiques.

## Intérêt Technologique et Innovations

Le framework REIO introduit une rupture méthodologique autour de trois axes :
1. **Alternative Asymétrique aux Multi-Cœurs en Lockstep :** Déporte la détection des fautes sur des sentinelles matérielles (< 50 Slice LUTs) et la reprise en ligne sur un superviseur en Rust bare-metal (22 mW).
2. **Émulation de Logique Trivalente (L₃) :** Évalue de manière déterministe un troisième état d'incertitude matérielle pour confiner localement les incohérences logiques.
3. **Isolation Hybride Synchrone / Combinatoire :** Combine une matrice de confinement à latence de 0 cycle et un étage de disjonction en 1 cycle contre les glitchs électriques.

## Spécifications et Intérêts Industriels de la Suite d'IP Cores

### REIO-CPU / TPU : Microprocesseur Ternaire Unifié (Safety Manager)
* **Spécification :** Séquenceur durci monolithique. Pilotage MMIO transparent (adresse `0x4000_6000`) depuis un logiciel Rust standard, sans compilateur ternaire dédié.
* **Intérêt Industriel :** Élimine le besoin de doubler intégralement le processeur (Lockstep matériel lourd). Il offre une isolation synchrone en 1 cycle d'horloge (10 ns) face aux dérives provoquées par des perturbations radiatives (MBU/SEU).
* **Cas d'Utilisation :** Utilisé comme contrôleur de sécurité central pour superviser en tâche de fond l'état des machines à états (FSM) critiques du calculateur hôte.

### REIO-DRIVE : Interface de Pilotage SÉCURISÉE pour Actionneurs
* **Spécification :** Interface de contrôle durcie pour actionneurs ADAS.
* **Intérêt Industriel :** Garantit un temps de propagation maximal déterministe pour empêcher l'injection d'ordres aberrants ou de pannes latentes au niveau de la couche physique des actionneurs du véhicule.
* **Cas d'Utilisation :** Placed directement en frontal des contrôleurs de moteurs de direction assistée ou des modules de freinage d'urgence autonome (AEB) pour valider la cohérence des trames de commande.

### REIO-SAFE : Sentinelle de Protection Mémoire (MMU Ultra-Light)
* **Spécification :** Sentinelle d'accès mémoire pour la protection des registres critiques.
* **Intérêt Industriel :** Apporte une isolation matérielle stricte à un coût de surface dérisoire, empêchant les attaques par débordement de tampon (buffer overflow) ou les pointeurs fous d'écraser la configuration de la puce.
* **Cas d'Utilisation :** Verrouille dynamiquement l'accès aux registres de configuration des horloges (Clock Gating) et de la gestion de l'alimentation après la phase de boot sécurisé.

### REIO-XBAR : Matrice d'Interconnexion et Confinement (Bus Guardian)
* **Spécification :** Commutateur réseau sur puce (NoC) et cellule de confinement de bus.
* **Intérêt Industriel :** Assure le cloisonnement des fautes (Fault Containment) en temps réel avec une latence combinatoire nulle (0 cycle). Si un composant non critique devient fou (*babbling idiot*), REIO-XBAR l'isole instantanément.
* **Cas d'Utilisation :** Positionné comme nœud central de communication entre le cœur de calcul applicatif (non sûr) et les périphériques certifiés ASIL-D.

Pour retrouver l'intégralité des tableaux comparatifs, des métrologies post-routage Vivado, ainsi que les détails complets de licence et de propriété intellectuelle (IEEE 1735), veuillez consulter le dépôt et les documents de référence associés.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20743411.svg)](https://doi.org/10.5281/zenodo.20743411)

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

## Licence et Propriété Intellectuelle

Ce framework est distribué sous un régime d'**Avis de Propriété Exclusive et Conditions d'Évaluation Publique**. Tous droits réservés © 2026. L'accès public à ce dépôt sert exclusivement de démonstration fonctionnelle et de portfolio de compétences.

Pour consulter l'intégralité des restrictions de reproduction et des conditions d'audit sous NDA, veuillez vous référer au fichier [LICENSE.md](LICENSE.md).

