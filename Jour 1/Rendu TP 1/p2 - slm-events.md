# Partie 2 — Pilotage du service (Service Level Management + Event Management)

## SLA / SLO proposés

- **SLA priorité critique** : première réponse sous 15 minutes, résolution sous 4 heures.
  **SLO** : 95% des tickets critiques respectent ce délai.
- **SLA priorité normale** : première réponse sous 2 heures, résolution sous 24 heures.
  **SLO** : 90% des tickets normaux respectent ce délai.

## Classification des logs

| Log | Classification | Justification | Action |
|---|---|---|---|
| `AUTH user=jdupont action=login status=success` | Informational | Connexion réussie, comportement normal | Aucune |
| `DISK host=SRV-FILE01 usage=82% threshold=80%` | Warning | Le seuil est dépassé mais le service fonctionne encore normalement | Planifier une purge des vieux fichiers ou augmenter l'espace disque |
| `SVC name=helpdesk-portal status=unreachable duration=00:04:12` | Exception | Le portail helpdesk est injoignable, ça touche directement tous les utilisateurs qui veulent ouvrir un ticket | Ouverture immédiate d'un Incident |
| `BACKUP job=nightly-backup host=SRV-DB01 status=completed` | Informational | Sauvegarde terminée sans erreur | Aucune |
| `NET link=switch-3F-port12 status=down flapping=true count=6/10min` | Exception | Le lien réseau clignote (monte/descend 6 fois en 10 min), c'est un signe de défaillance matérielle qui peut couper des utilisateurs à tout moment | Ouverture d'un Incident, intervention N2/N3 pour vérifier le port ou le câble
