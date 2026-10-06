# REIO — Framework de Co-Design Hardware/Software pour la Sûreté des Systèmes Embarqués

REIO (Réalisme Expérimental Instrumenté Optimisé) est un projet de recherche technologique indépendant (maturité TRL 4) adossé à un dépôt académique Zenodo. Ce framework propose une suite d'IP Cores d'accompagnement conçue pour durcir les architectures embarquées critiques à travers deux piliers micro-architecturaux majeurs : l'**émulation de logique trivalente par encodage matériel** et le déploiement de barrières d'**isolation active dynamique**.

## Architecture Générale et Approche Modulaire

Contrairement aux approches traditionnelles qui imposent une refonte complète des processeurs ou une redondance matérielle lourde, le framework REIO fonctionne selon un paradigme de co-design modulaire. Il ne remplace pas l'unité centrale hôte (ARM, RISC-V), mais déploie des automates matériels d'interception ultra-compacts couplés à une supervision logicielle légère.

* **Matériel (IP Cores Sentinelles) :** Des automates finis (FSM) combinatoires et séquentiels câblés directement au plus près des bus et des périphériques physiques. Ils surveillent l'intégrité des lignes logiques à la nanoseconde.
* **Logiciel (Superviseur Micro-noyau) :** Un micro-noyau écrit en Rust bare-metal running nativement sur le processeur hôte existant du client. En cas d'anomalie interceptée par le matériel, REIO isole la ligne en 1 cycle et lève une interruption immédiate, transférant la gestion de la reprise en ligne au superviseur logiciel.

## ## Description des Modules Spécifiques

### REIO-AMT (Matrice d'Interconnexion / Intercepteur de Bus)
* **Nature :** Brique périphérique sentinelle autonome et transparente.
* **Fonction :** Frontière d'isolation matérielle agissant comme un Bus Guardian. Structure configurée pour faciliter les analyses de vulnérabilité de niveau supérieur (type AVA_VAN.5).
* **Performances :** Latence de confinement de 0 cycle d'horloge (vitesse combinatoire pure). Consommation dynamique de 1 mW garantissant une excellente résistance à l'analyse différentielle de consommation (DPA).

### REIO-CPU (Automate Matériel Déterministe)
* **Nature :** Unité d'exécution et séquenceur durci autonome sous statut SEooC (Safety Element out of Context).
* **Fonction :** Automate d'état optimisé (FSM) émulant une algèbre ternaire déterministe pour l'analyse des opcodes critiques.
* **Performances :** Empreinte silicium optimisée à 20 Slice LUTs sous environnement AMD/Xilinx Vivado. Temps de réponse strict de 1 cycle d'horloge (10 ns à 100 MHz).

### REIO-DRIVE (Contrôleur d'I/O Actuateurs)
* **Nature :** Compagnon périphérique d'interfaçage physique.
* **Fonction :** Interface d'acquisition durcie pour la protection des bus de communication reliant les actionneurs électriques (ex: systèmes d'aide à la conduite ADAS).
* **Performances :** Empreinte restrictive de 21 Slice LUTs pour une consommation active maîtrisée à 3 mW sous stress nominal.

### REIO-SAFE (Cellule de Confinement / Storage Guard)
* **Nature :** Sentinelle compacte pour les accès mémoire.
* **Fonction :** Architecture d'**Isolation Active Dynamique** protégeant l'intégrité des blocs de stockage locaux et des registres de configuration contre les injections de fautes physiques.
* **Performances :** Empreinte minimale de 7 Slice LUTs et confinement synchrone des lignes d'autorisation d'écriture en 10 ns.

## Analyse Comparative et Métriques Physiques

Le tableau suivant positionne la suite d'IP Cores REIO face aux mécanismes classiques de sûreté de fonctionnement industrielle.

| Métrique Critique | Approches Standards (ARM / RISC-V Lockstep) | Suite d'IP Cores REIO |
| :--- | :--- | :--- |
| **Paradigme Logique** | Logique Booléenne Classique (0 / 1) | Émulation de logique trivalente par encodage matériel déterministe |
| **Temps de Réponse aux Fautes** | Plusieurs microsecondes (Gestion logicielle / Interruptions) | Déterministe : Confinement matériel en 1 cycle d'horloge strict (10 ns) |
| **Consommation Dynamique** | ~500 mW à plusieurs Watts | ~1 mW à 3 mW (Mesuré post-routage Vivado sur composants Slice Logic) |
| **Gestion de la Métastabilité** | Masquage classique par double échantillonnage | Synchronisation par bascules matérielles dédiées |
| **Confinement des Attaques / Glitchs** | Exception logicielle tardive (Crash ou gel système) | Isolation active immédiate (Bus Guardian combinatoire à 0 cycle) |
| **Surface d'Empreinte Silicium** | Redondance lourde (Duplication intégrale du cœur de calcul) | Ultra-compact : Implémentation restrictive (< 50 Slice LUTs au total) |
| **Cible Réglementaire Visée** | Certifications constructeurs dédiées | Conçu pour s'aligner sur les objectifs de sécurité ASIL-D (ISO 26262) |

## Statut du Projet et Conditions d'Évaluation

Le framework REIO est un projet de validation de concepts scientifiques. L'accès aux modèles de simulation fonctionnels opaques (`_funcsim.vhd`), aux Netlists chiffrées de synthèse industrielle (norme cryptographique IEEE 1735 via fichiers `.dcp` protégés pour Vivado), ainsi qu'aux matrices de traçabilité réglementaire (FMEDA et dossiers Compliance ISO 26262) est strictement réservé aux partenaires industriels ou académiques.

Toute demande d'évaluation technique ou d'audit des dossiers métrologiques consolidés est soumise à la signature préalable d'un Accord de Confidentialité mutuel (Mutual NDA) encadrant le Secret d'Affaires.

## Références Académiques

Le cadre théorique et les axiomes logiques régissant le modèle de traitement des incertitudes matérielles de REIO font l'objet d'un dépôt d'archive scientifique.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20743411.svg)](https://doi.org/10.5281/zenodo.20743411)

