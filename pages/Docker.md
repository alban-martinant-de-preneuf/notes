- ## installation ??
	- `sudo apt-get update && sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common`
	- `curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -`
	- `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"`
	- `sudo apt update && sudo apt-get install docker-ce docker-ce-cli containerd.io`
	- Ajouter l'utilisateur au groupe docker :
		- `sudo usermod -aG docker <user>`
- ## installation
	- (`sudo apt install docker.io docker-compose -y`)??
	- https://docs.docker.com/engine/install/ubuntu
	- docker compose est maintenant en plugin
- ## récupérer des images
	- https://hub.docker.com/
	- `docker pull <name>`
	-
	- ## utilisation
	- `docker images`
	- `docker run -it --name <instanceName> <name>`
	- `sudo docker run -it --name metasploitable -p <localPort>:<containerPort> -v <localFolder>:<containerFolder> meknisa/metasploitable-base /bin/bash`
	- quitter et éteindre
		- `exit`
	- quitter sans éteindre
		- ctrl+p ctrl+q
	- voir les conteneur instanciés
		- `docker ps -a`
	- lancer un conteneur
		- `docker start <instanceName>`
	- se connecter
		- `docker attach <instanceName>`
	- exécuter une commande
		- `docker exec -it <intanceName> <commande>`
	- supprimer un conteneur
		- `docker rm <name or id>`
	- supprimer une image
		- `docker image rm <name or id>`
	- Nettoyer le système
		- va supprimer les données suivantes :
		- l'ensemble des conteneurs Docker qui ne sont pas en status running ;
		- l'ensemble des réseaux créés par Docker qui ne sont pas utilisés par au moins un conteneur ;
		- l'ensemble des images Docker non utilisées ;
		- l'ensemble des caches utilisés pour la création d'images Docker.
		- `docker system prune`
- ## créer une image
	- Dans `Dockerfile` :
	- ![image.png](../assets/image_1701164318346_0.png)
	- Exemple
	- ```Dockerfile
	  FROM debian:9
	  
	  RUN apt-get update -yq \
	  && apt-get install curl gnupg -yq \
	  && curl -sL https://deb.nodesource.com/setup_10.x | bash \
	  && apt-get install nodejs -yq \
	  && apt-get clean -y
	  
	  ADD . /app/
	  WORKDIR /app
	  RUN npm install
	  
	  EXPOSE 2368
	  VOLUME /app/logs
	  
	  CMD npm run start
	  ```
	- Pour ignorer certains fichiers :
		- Ajouter un `dockerignore`
		- ```dockerfile
		  node_modules
		  .git
		  ```
	- `docker build -t <imageName> <DokerfileFolder>`
- ## Docker hub
	- Lier le local au distant
		- `docker tag <imageNameOrId>:latest <username>/<imageName>:latest`
		  id:: 65660081-5db0-4da0-8092-14bc75fbf4b8
	- Envoyer l'image
		- `docker push <username>/<imageName>:latest`
	- Rechercher sur docker hub
		- `docker search nginx`
- ## Docker-compose
	- ![image.png](../assets/image_1701189976833_0.png)
-
- ## nouvelle version
- executer une commande :
- `docker compose exec php /bin/sh`
- ## Sources
	- https://openclassrooms.com/fr/courses/2035766-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker/6211390-installez-docker-sur-votre-poste