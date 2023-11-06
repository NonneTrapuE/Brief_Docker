# Migration vers prestashop sur Docker

- Création du dossier /tmp/Migration et /etc/Migration/prestashop
- Sauvegarde de la base de données -> /tmp/Migration/dump.sql
- Export des fichiers présent dans /var/www/html vers /tmp/Migration/prestashop
- Téléchargement du (fichier)[https://raw.githubusercontent.com/NonneTrapuE/Brief_Docker/main/docker_ps/Dockerfile] et modifications si nécessaire (copie du fichier sql et restauration dans mariadb)
- Installation de Docker
- Docker build
- Arréter les services httpd, mariadb et php-fpm sur le serveur Linux
- Lancer le container Docker
- Si le conteneur est OK, supprimer le contenu du répertoire /var/www/html
- Désinstaller httpd, mariadb et les paquets php*
