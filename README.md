# Agent Global de Logging (Promtail)

Ce service est l'agent unique de collecte de logs pour tout le serveur.
Il tourne via Docker mais a accès aux logs de l'hôte et des autres conteneurs.

## Ce qu'il fait
1. **Docker Auto-Discovery** : Il se connecte au socket Docker. Dès qu'un nouveau conteneur démarre sur le serveur (via Dokploy ou manuellement), ses logs sont automatiquement aspirés.
2. **Logs Système** : Il lit `/var/log/fail2ban.log`, `/var/log/syslog`, etc.
3. **Envoi** : Il pousse tout vers l'instance Loki centralisée (`http://loki:3100`).

## Pré-requis
- Une stack Loki doit tourner sur le réseau `dokploy-network` avec le container_name `loki`.
- Le réseau `dokploy-network` doit exister.

## Installation dans Dokploy
1. Créez un service "Stack / Compose".
2. Connectez ce dépôt Git.
3. Déployez.

## Comment voir les logs ?
Dans Grafana > Explore :
- Pour une App Docker : `{container="nom-de-votre-app"}`
- Pour Fail2Ban : `{job="fail2ban"}`
- Pour le Système : `{job="varlogs"}`