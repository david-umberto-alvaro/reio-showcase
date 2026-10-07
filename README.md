# REIO — Framework de Co-Design Hardware/Software pour la Sûreté des Systèmes Embarqués

REIO (Réalisme Expérimental Instrumenté Optimisé) est un projet de recherche technologique indépendant (TRL 4) adossé à un dépôt académique Zenodo, proposant une suite d'IP Cores et un processeur de confiance pour durcir les architectures embarquées critiques.

## Intérêt Technologique et Innovations

Le framework REIO introduit une rupture méthodologique autour de trois axes :
1. **Alternative Asymétrique aux Multi-Cœurs en Lockstep :** Déporte la détection des fautes sur des sentinelles matérielles (< 50 Slice LUTs) et la reprise en ligne sur un superviseur en Rust bare-metal (22 mW).
2. **Émulation de Logique Trivalente (L₃) :** Évalue de manière déterministe un troisième état d'incertitude matérielle pour confiner localement les incohérences logiques.
3. **Isolation Hybride Synchrone / Combinatoire :** Combine une matrice de confinement à latence de 0 cycle et un étage de disjonction en 1 cycle contre les glitchs électriques.

## Spécifications de la Suite d'IP Cores

* **REIO-CPU / TPU :** Microprocesseur ternaire unifié (67 Slice LUTs, 157 Slice Registers sur AMD Artix-7). Utilise un pilotage MMIO standard (adresse `0x4000_6000`) depuis un logiciel Rust sans nécessiter de compilateur ternaire natif. Isolation en 1 cycle d'horloge (ISO 26262 ASIL-D).
* **REIO-DRIVE :** Interface de pilotage durcie pour actionneurs ADAS (24 Slice LUTs / 19 Slice Registers) garantissant un temps de propagation maximal déterministe.
* **REIO-SAFE :** Sentinelle de protection mémoire ultra-légère (8 Slice LUTs / 13 Slice Registers) pour empêcher les débordements de tampon.
* **REIO-XBAR :** Matrice d'interconnexion et Bus Guardian combinatoire (22 LUTs) assurant un cloisonnement des fautes en temps réel.

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

