# TP ITIL 5 — Amélioration d'un service (helpdesk interne)

Cas traité : PME fictive **NovaTech Solutions**, ~180 salariés, helpdesk interne organisé en 5 techniciens N1 / 2 techniciens N2 / 1 technicien N3.

Ce README sert de compte rendu unique et regroupe le contenu de toutes les parties du TP.

---

## Sommaire des fichiers du dépôt

| Fichier | Contenu |
|---|---|
| `p1-csi-register.md` | Partie 1 — Diagnostic du service sous les 4 dimensions ITIL, CSI Register (3 améliorations priorisées), principe directeur mobilisé |
| `p2-slm-events.md` | Partie 2 — 2 SLA/SLO proposés, classification des 5 logs fournis (Informational/Warning/Exception) et actions associées |
| `p3-change-kb.md` / `p3-change-kb.txt` | Partie 3 — RFC complète (type, impact, rollback, validation CAB simulée), article de base de connaissance, positionnement dans le Product and Service Lifecycle |
| `p4-service-request.md` / `p4-service-request.txt` | Partie 4 — Ticket GLPI complet : formulaire principal, journal de suivi chronologique, tâches de vérification, solution de clôture |
| `bonus-escalade.md` | Bonus — matrice d'escalade N1/N2/N3 et schéma du parcours d'un ticket |
| `logs.txt` | Ressource fournie par l'énoncé (5 logs à classer, utilisés en Partie 2) |

Les versions `.txt` de la Partie 3 et de la Partie 4 sont des copies sans mise en forme markdown, faites pour être collées directement dans les champs de l'outil GLPI.

---

## Partie 1 — Diagnostic (4 dimensions + Continual Improvement)

### Constats par dimension

- **Organisations & personnes** : les 5 N1 sont souvent débordés entre 9h et 10h (pic des demandes du matin), il n'y a pas de tournante organisée, et rien n'oblige un N1 à transmettre un ticket au N2 si le sujet dépasse ses compétences. Résultat : certains tickets restent bloqués chez un N1 plusieurs jours sans que le N2 soit au courant.
- **Information & technologie** : l'outil de ticketing (GLPI) est utilisé mais aucune règle d'escalade automatique n'est configurée. Il n'y a pas non plus de base de connaissance partagée entre les 5 N1, donc chacun redemande les mêmes informations à l'utilisateur, ce qui explique les rappels multiples.
- **Partenaires & fournisseurs** : le fournisseur qui gère le parc d'imprimantes met en moyenne 3 à 5 jours pour intervenir sur une panne matérielle, et il n'existe aucun engagement de délai (SLA) signé avec lui.
- **Value Streams & processus** : il n'y a pas de procédure écrite pour qualifier un ticket dès son arrivée (priorité, catégorie). Du coup, un ticket urgent peut être classé "normal" par erreur et attendre son tour dans la file, ce qui ralentit tout le traitement.

### CSI Register

| Amélioration | Effort | Impact | Priorité |
|---|---|---|---|
| Configurer des règles d'escalade automatique dans GLPI (un ticket non traité après X heures remonte automatiquement au N2) | Faible | Fort | 1 |
| Créer une base de connaissance partagée pour les 5 N1 (FAQ des demandes les plus fréquentes) | Faible | Moyen | 2 |
| Renégocier le contrat avec le fournisseur d'imprimantes pour fixer un SLA d'intervention | Fort | Fort | 3 |

### Principe directeur mobilisé

**"Progresser de manière itérative avec du feedback"**. On commence par la règle d'escalade automatique dans GLPI, parce que c'est rapide à mettre en place et que ça corrige tout de suite le problème le plus gênant (les tickets qui restent bloqués sans que personne ne s'en rende compte). La renégociation avec le fournisseur d'imprimantes est plus lourde à mener, donc on préfère la lancer après avoir observé si les premiers changements suffisent déjà à réduire les plaintes.

---

## Partie 2 — Pilotage du service (Service Level Management + Event Management)

### SLA / SLO proposés

- **SLA priorité critique** : première réponse sous 15 minutes, résolution sous 4 heures. **SLO** : 95% des tickets critiques respectent ce délai.
- **SLA priorité normale** : première réponse sous 2 heures, résolution sous 24 heures. **SLO** : 90% des tickets normaux respectent ce délai.

### Classification des logs

| Log | Classification | Justification | Action |
|---|---|---|---|
| `AUTH user=jdupont action=login status=success` | Informational | Connexion réussie, comportement normal | Aucune |
| `DISK host=SRV-FILE01 usage=82% threshold=80%` | Warning | Seuil dépassé mais service encore fonctionnel | Planifier une purge ou augmenter l'espace disque |
| `SVC name=helpdesk-portal status=unreachable duration=00:04:12` | Exception | Portail helpdesk injoignable, impact direct sur tous les utilisateurs | Ouverture immédiate d'un Incident |
| `BACKUP job=nightly-backup host=SRV-DB01 status=completed` | Informational | Sauvegarde terminée sans erreur | Aucune |
| `NET link=switch-3F-port12 status=down flapping=true count=6/10min` | Exception | Lien réseau qui clignote, signe de défaillance matérielle pouvant couper des utilisateurs | Ouverture d'un Incident, intervention N2/N3 |

---

## Partie 3 — Traitement du changement (Change Enablement + Knowledge Management)

### RFC — Configuration des règles d'escalade automatique dans GLPI

- **Type** : Normal (ça change les habitudes de travail des N1/N2, donc à évaluer, mais pas urgent ni très risqué)
- **Impact** : les 5 N1 et les 2 N2 (nouvelles notifications à gérer). Risque : une règle mal réglée pourrait surcharger les N2.
- **Plan de rollback** : l'ancienne escalade manuelle reste active 2 semaines en parallèle ; en cas de souci, on désactive simplement la règle dans GLPI.
- **Validation CAB simulée** :
  - Demandeur : "On perd des tickets depuis des mois, il faut corriger rapidement."
  - Approbateur : "D'accord sur le principe, mais on teste d'abord sur le service comptabilité une semaine."

### Article de base de connaissance

- **Symptôme** : un ticket reste assigné à un N1 plusieurs jours sans que le N2 soit prévenu, alors que le sujet le dépasse.
- **Cause** : absence de règle d'escalade automatique dans GLPI.
- **Résolution** : règle GLPI qui escalade automatiquement selon un délai défini par priorité.
- **Mots-clés** : escalade, GLPI, ticket bloqué, N1, N2

### Positionnement dans le Product and Service Lifecycle

Ce changement mobilise surtout **Build** (configuration de la règle) et **Transition** (formation des N1/N2), avec un chevauchement sur **Operate** (observation des premiers jours en conditions réelles pour ajuster). Ça montre que ce modèle en 8 étapes n'est pas linéaire : les étapes se chevauchent selon le contexte.

*(Détail complet : voir `p3-change-kb.md`)*

---

## Partie 4 — Clôture (Service Request Management)

Un ticket GLPI a été traité pour déclencher la mise en œuvre du changement de la Partie 3 : demande de configuration de la règle d'escalade, transmise du responsable helpdesk au technicien N2. Le ticket documente un journal chronologique complet (prise en charge, tests, une anomalie de seuil rencontrée et corrigée, déploiement pilote, suivi), des tâches de vérification cochées avant clôture, et une solution finale résumant le résultat.

*(Détail complet du ticket, prêt à coller dans GLPI : voir `p4-service-request.md` / `.txt`)*

---

## Synthèse générale

| Partie | Pratique(s) mobilisée(s) |
|---|---|
| 1 | Continual Improvement |
| 2 | Service Level Management, Event Management |
| 3 | Change Enablement, Knowledge Management |
| 4 | Service Request Management |

### Principe directeur le plus structurant

**"Progresser de manière itérative avec du feedback"**. On le retrouve dès la Partie 1 : plutôt que de tout changer d'un coup (renégocier le fournisseur, refaire tout le processus), on commence par la règle d'escalade GLPI, la plus rapide à mettre en place, et on observe le résultat avant d'aller plus loin. On garde la même logique en Partie 3 : la RFC prévoit un test sur un seul service (comptabilité) avant un déploiement à tout le monde.

### AI Governance / 6C

Dans l'état actuel du cas, le module AI Governance n'est pas directement mobilisé : aucune décision automatisée n'est en jeu, la règle d'escalade GLPI est juste une règle fixe (si délai dépassé → escalade), pas une IA qui décide.

Par contre, si NovaTech allait plus loin en ajoutant un assistant IA pour catégoriser automatiquement les tickets à l'arrivée (deviner la priorité et le service concerné), le modèle **6C** deviendrait pertinent : il faudrait définir qui valide les catégorisations de l'IA en cas d'erreur (un ticket mal classé "normal" alors qu'il est critique), et garder un contrôle humain sur les cas ambigus plutôt que de laisser l'IA décider seule.

---

## Bonus

Une matrice d'escalade N1/N2/N3 et un schéma du parcours d'un ticket sont disponibles dans `bonus-escalade.md`.
