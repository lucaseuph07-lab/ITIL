# TP ITIL/GLPI — "Une journée chez NORTEK SI"

Compte rendu récapitulatif des 5 phases traitées dans GLPI. Entreprise fictive NORTEK SI, 200 salariés, DSI de 4 personnes.

---

## Phase 1 — Gestion des incidents

Les 5 tickets ont été créés dans GLPI et priorisés selon la matrice impact/urgence, pas dans l'ordre d'arrivée.

| Ticket | Sujet | Priorité | Justification |
|---|---|---|---|
| Lucas 2 | Serveur de messagerie ne répond plus (toute l'entreprise) | Haute | Impact maximal (200 salariés touchés), urgence haute → traité en premier |
| Lucas 1 | Email suspect demandant des identifiants | Haute | Ce n'est pas un incident technique classique mais une tentative de phishing : risque de compromission de comptes si un autre utilisateur clique, donc traité en priorité juste après le mail, avant les pannes matérielles |
| Lucas 3 | DAF (VIP) ne peut plus ouvrir son tableau de bord financier | Haute | Utilisateur VIP + donnée financière sensible, même si une seule personne est impactée |
| Lucas 4 | Imprimante du 2e étage hors service | Haute | Un service entier impacté, et lié à une intervention réseau récente (voir Phase 2) |
| Lucas 5 | Souris ne fonctionne plus (comptabilité) | Basse | Un seul utilisateur, panne matérielle mineure sans urgence |

Ordre de traitement retenu : Lucas 2 → Lucas 1 → Lucas 3 → Lucas 4 → Lucas 5.

---

## Phase 2 — Gestion des problèmes

Un Problème unique a été créé dans GLPI, regroupant les tickets **Lucas 2** (serveur mail) et **Lucas 4** (imprimante 2e étage), tous deux liés à une même intervention réseau réalisée par le prestataire NordLog au 2e étage.

**Méthode des 5 pourquoi**
1. Pourquoi le serveur mail et l'imprimante sont tombés en panne ? Une intervention réseau a été réalisée peu avant sur le 2e étage.
2. Pourquoi cette intervention a-t-elle eu un tel impact ? Elle a modifié la configuration d'un switch partagé par plusieurs services.
3. Pourquoi cette modification a-t-elle touché des équipements hors du périmètre prévu ? Ce switch dessert plusieurs VLAN sur plusieurs étages sans cloisonnement clair.
4. Pourquoi ce périmètre réel n'a-t-il pas été anticipé avant l'intervention ? La CMDB ne documentait pas les dépendances entre ce switch et les équipements des autres étages.
5. Pourquoi la CMDB n'était-elle pas à jour ? Il n'existait pas de processus systématique de mise à jour de la CMDB après chaque intervention réseau.

**Workaround immédiat** : en attendant la correction définitive côté réseau, un accès de secours a été proposé pour la messagerie (webmail) et l'imprimante voisine a été désignée comme solution de repli pour le service impacté.

**Entrée KEDB** : Symptôme — coupure simultanée du serveur mail et perte de détection réseau d'une imprimante ; Cause — mauvaise configuration réseau (VLAN/port) lors d'une intervention prestataire ; Solution connue — vérifier la configuration du switch concerné et la documentation CMDB avant toute nouvelle intervention sur ce point réseau.

---

## Phase 3 — Gestion des changements

Une RFC a été rédigée pour corriger la configuration réseau du switch du 2e étage, cause racine identifiée en Phase 2.

- **Type** : Normal (cause identifiée, workaround déjà en place, intervention planifiable sereinement)
- **Impact** : les 200 salariés (messagerie) + le service concerné par l'imprimante
- **Plan de rollback** : conservation de la configuration actuelle avant modification, retour en arrière possible en cas d'échec
- **Fenêtre de maintenance** : soirée en semaine, hors heures de bureau, en coordination avec NordLog
- **Validation CAB simulée** : avis favorable, sous réserve que NordLog confirme la configuration cible et qu'un technicien N2 interne valide après l'intervention

Ce changement a été lié dans GLPI au Problème unique de la Phase 2 ainsi qu'aux deux incidents concernés (Lucas 2 et Lucas 4).

---

## Phase 4 — Gestion des configurations (CMDB)

La fiche CI du switch du 2e étage a été créée, avec ses CI dépendants documentés : le serveur de messagerie, l'imprimante du 1er étage, et les postes clients du même VLAN.

En simulant l'impact avant intervention, une anomalie a été découverte : un boîtier de téléphonie IP du 2e étage est câblé sur le même port/VLAN, sans avoir été identifié lors de l'intervention initiale. Ce CI a été ajouté à la liste des dépendances, et le service concerné a été prévenu du risque de coupure téléphonique en plus du mail et de l'imprimante lors d'une prochaine intervention. Ce cas illustre l'intérêt concret d'une CMDB à jour : sans elle, ce périmètre d'impact réel serait resté invisible.

---

## Phase 5 — Gestion des demandes de service

Une Demande de service distincte des incidents a été créée pour l'arrivée de Julie (chargée de communication), prévue lundi prochain : création du compte utilisateur et de la boîte mail, déploiement d'un poste complet avec périphériques, accès VPN, et accès au dossier partagé "Communication".

Contrairement à un incident traité en quelques heures, cette demande suit un SLA de demande de service de 3 à 5 jours ouvrés. La date cible de mise à disposition a été fixée au lundi matin, jour d'arrivée de Julie, ce qui reste dans les délais du SLA.

---

## Cohérence globale du fil rouge

L'ensemble des 5 phases s'enchaîne autour d'un même événement déclencheur : l'intervention réseau du 2e étage. Les incidents de la Phase 1 (mail, imprimante) sont regroupés dans le Problème de la Phase 2, dont la cause racine débouche sur le changement de la Phase 3, lui-même éclairé par l'analyse d'impact de la Phase 4. La Phase 5 reste un fil indépendant (demande de service planifiée), traité en parallèle sans lien avec l'incident réseau, ce qui montre la distinction entre gestion des incidents/problèmes et gestion des demandes de service.
