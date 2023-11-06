# Brief_docker

## Procédure HomeAssistant Rocky Linux 9

### Installer Docker 

```
dnf install docker-ce docker-ce-cli containerd.io
```

### Créer un utilisateur et l'ajouter au groupe docker

```
useradd utilisateur --shell /bin/bash --create-home -groups wheel,docker
passwd utilisateur
```

### Créer un répertoire

```
mkdir /home/utilisateur/conf
```

### Configuration de SELinux

```
getenforce        # Si Enforcing
setenforce 0
```

### Créer le conteneur

```
docker run -d --name homeassistant --net=host --privileged --restart=unless-stopped -v /home/utilisateur/conf:/config -e TZ=Europe/Paris ghcr.io/home-assistant/home-assistant:stable
```

### Configurer le port 

```
firewall-cmd --add-port=8123/tcp --permanent
firewall-cmd --reload
```

### Vérifier que le conteneur soit lancé 

```
docker ps -a
```

### Arréter le conteneur 


```
docker kill containerID
```

### Supprimer le conteneur 


```
docker rm containerID
```

--- 

## Procédure pour le conteneur Fedora_base et le conteneur Fedora_httpd

* Créer un dossier

```mkdir fedora_base```

* Télécharger le fichier Dockerfile

```cd fedora_base && wget https://raw.githubusercontent.com/NonneTrapuE/Brief_docker/main/fedora_base.Dockerfile```

* Renommer le fichier

```mv fedora_base.Dockerfile Dockerfile```

* Builder le projet :

```docker build -t fedora_base .```

* Lancer le conteneur

```docker run -d --privileged --volume /sys/fs/cgroup:/sys/fs/cgroup fedora_base```

* Pour le conteneur fedora_httpd, ajoutez l'option -p afin de rediriger le port 80. Il est également nécessaire d'activer le service httpd dans le conteneur. Pour ceci :

``` docker exec -ti containerID bash```

* Une fois dans le conteneur :

```systemctl enable --now httpd```

Il s'agit ici de conteneurs expérimentaux, ne pas utiliser en production pour des soucis de sécurité (accès en lecture/écriture aux [cgroups](https://fr.wikipedia.org/wiki/Cgroups))).
Si on modifie l'accès au cgroups pour le placer en Read-Only, les conteneurs ne démarrent pas. 

### Pourquoi créer un conteneur ?

L'image Docker de Fedora n'intègre pas systemd et est donc inutilisable dans mon cas. (installation de plusieurs processus dans un conteneur)


