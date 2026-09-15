# Partie 1 — Diagnostic (4 dimensions + Continual Improvement)

## Contexte du cas (imaginaire)

On a choisi une PME fictive : **NovaTech Solutions**, environ 180 salariés, un seul site principal.
Le service helpdesk interne compte :

- **5 techniciens N1** (premier contact : mots de passe, comptes, imprimantes, petits soucis matériels)
- **2 techniciens N2** (problèmes plus techniques : réseau, serveurs applicatifs, comptes AD complexes)
- **1 technicien N3** (expert infrastructure/sécurité, dernier niveau avant de contacter un fournisseur externe)

Les plaintes remontées : lenteur de traitement, tickets perdus, utilisateurs qui rappellent plusieurs fois pour le même souci.

## Constats par dimension

- **Organisations & personnes** : les 5 N1 sont souvent débordés entre 9h et 10h (pic des demandes du matin), il n'y a pas de tournante organisée, et rien n'oblige un N1 à transmettre un ticket au N2 si le sujet dépasse ses compétences. Résultat : certains tickets restent bloqués chez un N1 plusieurs jours sans que le N2 soit au courant.
- **Information & technologie** : l'outil de ticketing (GLPI) est utilisé mais aucune règle d'escalade automatique n'est configurée. Il n'y a pas non plus de base de connaissance partagée entre les 5 N1, donc chacun redemande les mêmes informations à l'utilisateur, ce qui explique les rappels multiples.
- **Partenaires & fournisseurs** : le fournisseur qui gère le parc d'imprimantes met en moyenne 3 à 5 jours pour intervenir sur une panne matérielle, et il n'existe aucun engagement de délai (SLA) signé avec lui.
- **Value Streams & processus** : il n'y a pas de procédure écrite pour qualifier un ticket dès son arrivée (priorité, catégorie). Du coup, un ticket urgent peut être classé "normal" par erreur et attendre son tour dans la file, ce qui ralentit tout le traitement.

## CSI Register

| Amélioration | Effort | Impact | Priorité |
|---|---|---|---|
| Configurer des règles d'escalade automatique dans GLPI (un ticket non traité après X heures remonte automatiquement au N2) | Faible | Fort | 1 |
| Créer une base de connaissance partagée pour les 5 N1 (FAQ des demandes les plus fréquentes) | Faible | Moyen | 2 |
| Renégocier le contrat avec le fournisseur d'imprimantes pour fixer un SLA d'intervention | Fort | Fort | 3 |

## Principe directeur mobilisé

On s'est appuyé sur **"Progresser de manière itérative avec du feedback"**. On commence par la règle d'escalade automatique dans GLPI, parce que c'est rapide à mettre en place et que ça corrige tout de suite le problème le plus gênant (les tickets qui restent bloqués sans que personne ne s'en rende compte). La renégociation avec le fournisseur d'imprimantes est plus lourde à mener (négociation contractuelle), donc on préfère la lancer après avoir observé si les premiers changements suffisent déjà à réduire les plaintes.
