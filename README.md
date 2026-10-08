# REIO — Framework de Co-Design Hardware/Software pour la Sûreté des Systèmes Embarqués

REIO (Réalisme Expérimental Instrumenté Optimisé) est un projet de recherche technologique indépendant (TRL 4) adossé à un dépôt académique Zenodo, proposant une suite d'IP Cores et un processeur de confiance pour durcir les architectures embarquées critiques.

## Intérêt Technologique et Innovations

Le framework REIO introduit une rupture micro-architecturale autour de trois axes :

1. **Alternative Asymétrique Économique au Lockstep :** Supprime la duplication lourde de processeurs physiques standard. Il déporte la tolérance aux pannes sur des micro-moteurs asymétriques monolithiques unifiés et la gestion de crise sur un superviseur en Rust bare-metal, divisant par trois l'empreinte silicium.
2. **Co-Design Paraconsistant et IA Neuromorphique :** Émule une logique trivalente native (L₃) pour évaluer les états d'incertitude physique et intègre un accélérateur à impulsions événementiel (SNN) agissant comme une sentinelle de calcul autonome.
3. **Disjonction Étanche et Annihilation en 1 Cycle :** Combine un Bus Guardian combinatoire à latence de 0 cycle et un étage de pipeline périphérique synchrone pour isoler la puce en 10 ns, forçant le drainage immédiat des bus vers le potentiel neutre de la masse (0V).

## Spécifications et Intérêts Industriels de la Suite d'IP Cores

L'infrastructure matérielle REIO s'articule autour d'une suite de blocs de silicium spécialisés, conçus pour s'interconnecter de manière transparente au sein d'une architecture système sécurisée :

| IP Core REIO | Rôle Micro-Architectural | Intérêt Industriel Stratégique | Cas d'Utilisation Système |
| :--- | :--- | :--- | :--- |
| **CPU / TPU** | Microprocesseur Ternaire Unifié (Safety Manager) | Élimine le besoin de doubler intégralement le processeur (Lockstep lourd). Offre une isolation synchrone en 1 cycle (10 ns) face aux dérives radiatives (MBU/SEU). Pilotage MMIO standard (`0x4000_6000`) en Rust. | Contrôleur de sécurité central pour superviser en tâche de fond l'état des machines à états (FSM) critiques du calculateur hôte. |
| **DRIVE** | Interface de Pilotage pour Actionneurs | Garantit un temps de propagation maximal déterministe pour empêcher l'injection d'ordres aberrants ou de pannes latentes sur la couche physique. | Positionné en frontal des contrôleurs de moteurs de direction assistée ou de freinage d'urgence autonome (AEB) pour valider la cohérence. |
| **XBAR** | Matrice d'Interconnexion (Bus Guardian) | Assure le cloisonnement des fautes (Fault Containment) en temps réel avec une latence combinatoire nulle (0 cycle). Isole instantanément un nœud défaillant. | Nœud central de communication sécurisé entre le cœur de calcul applicatif (non sûr) et les périphériques certifiés ASIL-D. |
| **NEXUS** | Supercalculateur Monolithique Unifié (CPU + SNN) | Élimine le besoin de doubler intégralement le processeur (Lockstep lourd). Fusionne un cœur ternaire paraconsistant et un accélérateur d'IA neuromorphique (SNN). Consommation active de seulement 30 mW. | Cœur de contrôle adaptatif et d'intelligence artificielle événementielle pour la détection en ligne de scénarios critiques. |
| **SRAM** | Banque de Stockage Durcie 16 bits | Apporte une protection continue face aux rayonnements ionisants (SEU) avec un inspecteur de parité combinatoire en 0 cycle. | Zone de rétention et de mise en cache ultra-sûre pour stocker les variables d'état métier du véhicule sans surcharge ECC logicielle. |
| **BIST** | Sentinelle d'Auto-Test Périodique | Garantit la détection active des pannes latentes et dormantes en injectant des stimuli cycliques en tâche de fond (0 Latch). | Module d'audit matériel autonome pour certifier à chaque cycle que les mécanismes et disjoncteurs de sécurité ne sont pas en panne. |

## Analyse Comparative Globale

| Métrique Critique | Approches Standards (ARM / RISC-V Lockstep) | Écosystème Intégral REIO-NEXUS |
| :--- | :--- | :--- |
| **Paradigme Logique** | Logique Booléenne Classique (0 / 1) | Émulation de logique trivalente par encodage matériel déterministe (L₃). |
| **Temps de Réponse aux Fautes** | Plusieurs microsecondes (Surcharge logicielle) | Déterministe : Confinement matériel en 1 cycle d'horloge (10 ns). |
| **Consommation Dynamique** | ~500 mW à plusieurs Watts | **Ultra-sobre : ~30 mW à 80 mW** (Pleine charge active post-routage d'usine). |
| **Surface d'Empreinte Silicium** | Redondance matérielle lourde (Duplication intégrale) | **Ultra-compact : < 800 Slice LUTs au total** pour le SoC complet (NEXUS + SRAM + BIST). |
| **Cible Réglementaire Visée** | Certifications constructeurs génériques | Conçu pour s'aligner sur les exigences maximales **ASIL-D (ISO 26262)**. |

## Statut et Références

Le projet est en validation de concepts (TRL 4). L'accès complet aux netlists, simulations et dossiers de conformité est réservé aux partenaires sous NDA.
### 🔬 Fondations Théoriques & Spécifications (Zenodo DOI)

* **REIO-CORE :** Cadre logique formel s'appuyant sur une approche logique paraconsistante et des machines d'états (FSM) durcies pour garantir un confinement contextuel déterministe malgré les fautes physiques (*bit-flips*). Document de recherche officiel enregistré sous l'identifiant académique permanent : [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20743411.svg)](https://doi.org/10.5281/zenodo.20743411)

### Matrice d'Alignement Synthétique (Fondations Logiques L₃ ⇄ IP Cores)

L'infrastructure matérielle implémentée sous Vivado est la traduction physique directe des règles de sûreté formalisées dans la théorie paraconsistante :

| Axiome | Pilier Théorique | Implémentation Hardware (Suite REIO) | Impact sur la Sûreté Réelle |
| :--- | :--- | :--- | :--- |
| **REIO-A1** | Ancrage Matériel Pur | **REIO-NEXUS** (Cœur CPU) | Confinement strict par exclusion d'états intermédiaires. Bloque l'erreur en matériel sans saturer le processeur hôte. |
| **REIO-A2** | Isolation des Perceptions | **REIO-DRIVE** | Exclusion totale de l'intervention humaine pour prémunir les registres d'actionneurs de toute altération. |
| **REIO-A3** | Convergence Orthogonale | **REIO-XBAR** | Filtrage matériel ternaire en ligne. Rejet immédiat de toute donnée non ancrée aux primitives physiques (Résolution de Gettier). |
| **REIO-A4** | Confinement & Seuils | **REIO-XBAR** (Bus Guardian) | Disjonction physique instantanée en 1 cycle (10 ns) dès le franchissement des seuils critiques pour découpler les bus corrompus. |
| **REIO-A5** | Axiomatisation Récursive | **REIO-SRAM** | Élimination mathématique de la métastabilité inter-horloges par ajustement discret (+1, -1, 0) pour garantir la persistance mémoire. |
| **REIO-A6** | Attestation Pragmatique | **REIO-NEXUS** (Cœur SNN / BIST) | Scellement irréversible de chaque cycle d'évolution et auto-test cyclique des pannes dormantes. Interception immédiate des fautes. |


## Licence et Propriété Intellectuelle

Ce framework est distribué sous un régime d'**Avis de Propriété Exclusive et Conditions d'Évaluation Publique**. Tous droits réservés © 2026. L'accès public à ce dépôt sert exclusivement de démonstration fonctionnelle et de portfolio de compétences.

Pour consulter l'intégralité des restrictions de reproduction et des conditions d'audit sous NDA, veuillez vous référer au fichier [LICENSE.md](LICENSE.md).

