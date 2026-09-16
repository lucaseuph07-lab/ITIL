# Partie 3 — Traitement du changement (Change Enablement + Knowledge Management + Product and Service Lifecycle)

## RFC — Configuration des règles d'escalade automatique dans GLPI

- **Type** : Normal (ça change les habitudes de travail des 5 N1 et des 2 N2, donc ça mérite d'être évalué avant d'être déployé, mais ce n'est pas urgent ni très risqué).
- **Impact** : concerne les 5 techniciens N1 et les 2 techniciens N2 (nouvelles notifications à gérer). Risque principal : une règle mal réglée pourrait escalader trop de tickets d'un coup et surcharger les N2 au lieu de les aider.
- **Plan de rollback** : on garde l'ancien fonctionnement (escalade manuelle) actif en parallèle pendant 2 semaines. Si la règle automatique pose trop de problèmes, on la désactive dans GLPI et tout redevient comme avant, sans perte de données.
- **Validation CAB simulée** :
  - Demandeur : "On perd des tickets depuis des mois à cause de l'absence d'escalade automatique, il faut le corriger rapidement."
  - Approbateur : "D'accord sur le principe, mais on teste d'abord sur les tickets du service comptabilité pendant une semaine avant de l'appliquer à tout le monde."

## Article de base de connaissance

- **Symptôme** : un ticket reste assigné à un technicien N1 pendant plusieurs jours sans qu'aucun N2 ne soit prévenu, alors que le sujet dépasse le niveau N1.
- **Cause** : absence de règle d'escalade automatique dans GLPI ; l'escalade dépend uniquement de l'initiative du technicien N1.
- **Résolution** : mise en place d'une règle GLPI qui escalade automatiquement un ticket vers le N2 s'il reste sans mise à jour au-delà d'un délai défini selon sa priorité.
- **Mots-clés** : escalade, GLPI, ticket bloqué, N1, N2

## Positionnement dans le Product and Service Lifecycle

Ce changement mobilise surtout les étapes **Build** (configuration de la règle d'escalade dans GLPI) et **Transition** (annonce et petite formation des N1/N2 sur le nouveau fonctionnement). On peut aussi dire que ça touche un peu l'étape **Operate**, parce que l'équipe va observer les premiers jours en conditions réelles pour ajuster les délais si besoin. Ça montre bien que ce modèle en 8 étapes n'est pas une simple ligne droite : ici, Build/Transition et Operate se chevauchent, on n'attend pas d'avoir totalement fini une étape avant de commencer la suivante.
