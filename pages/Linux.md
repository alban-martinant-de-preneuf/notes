- # Linux
	- Les fichiers sont représentés dans le noyau Linux par des structures nommées inode.
	- Les opérations classiques de Copie/Déplacement/Suppression de fichier sous Linux sont exécutées sur les inodes avec les commandes `cp/mv/rm`
	- SetUID est un droit spécial permettant d'exécuter un fichier avec les droits de son propriétaire
	- Le droit d'exécution sur un dossier donne le droit de le "traverser".
	- Pour changer le droits donnés par défaut lors de la création d'un fichier ou d"un répertoire, il faut modifier le "UMASK"; c'est la valeur à soustraire aux bit des droit, 666 pour le fichiers et  777 pour les dossiers. Par défaut le UMASK est à 022 donc les droits à 755 pour les dossier et 644 pour les fichiers.
	- ## Aide
		- `<commande> --help`
		- Pour les commandes primitives(ou internes) (`help` pour les lister toutes):
			- `help <commande>`
		- Pour les commandes externes :
			- `man <commande>` est plus détaillé
	-
	- ## Commandes
		- Pour connaitre le type d'une commande :
			- `type <commande>`
		- Informations sur un fichier :
			- `file <fichier>`
		- `history` pour l'historique des commandes
		- `!<historyNb>`
		- `cat -n <file>` : affiche le flux passé en entrée avec les numéros des lignes.
		- Navigation et astuces pour `less`
			- G aller à la fin du fichier
			- g aller au début du fichier
			- <backspace> page suivante
			- b pour "back" revenir en arrière
			- -N <enter> afficher les lignes
				- <n> pour aller à la ligne "n"
			- / rechercher, n pour "next" N pour précédent
		- `sed`(stream editor) est utilisé effectuer des modifications de texte telles que des recherches et des remplacements, des suppressions de lignes, des insertions et des opérations de formatage :
			- Rechercher et remplacer un mot ou une expression :
				- `sed 's/mot_a_chercher/mot_de_remplacement/g' fichier.txt`
			- Supprimer des lignes contenant un mot ou une expression :
				- `sed '/mot_a_chercher/d' fichier.txt`
			- Ajouter une ligne après une ligne spécifique :
				- `sed 'N;/mot_a_chercher/a\ Nouvelle_ligne' fichier.txt`
			- Formater un fichier en supprimant les espaces inutiles :
				- `sed 's/ \+//g' fichier.txt`
			- Afficher uniquement les lignes compris entre deux lignes spécifiques :
				- `sed -n '/debut/,/fin/p' fichier.txt`
		- `awk` fait des actions similaire à `sed`
		- `cut -d , -f 1` ou `-d`définit le délimiteur et `-f` le field
		- `sort` pour trier
		- `ls` options utiles :
			- `-i` inode
			- `-t`classer par date
			- `-r` inverser l'ordre
			- `-R` récursif
			- `-h` human readable
			- `-l`long
				- droits - occurence de l'inode - owner - group owner - taille - modif date - name
		- `mv fichier1 fichier2` déplacer/renommer (le fichier garde le même inode)
		- Chercher toutes les occurences d'un inode :
			- `find / -inum <numéro_inode>`
		- `ln file1 flie2`crée un lien, les 2 fichiers on le même inode
		- `ln -s file1 file2`lien symbolique, file1 va être un inode à part entière qui contient l'adresse de flle2
		- Ajouter un utilisateur à un groupe :
			- `sudo gpasswd -a user group`
			- `sudo usermod -a -G group user`
		- Trouver les 50 fichiers les plus volumineux :
			- `sudo find / -type f -exec du -Sh {} + | sort -hr | head -50`
		- `grep -B 3 -A 2 foo README.txt` pour le contexte -B pour before -A pour after
		- La commande `dmesg` est utilisée pour afficher les messages liés au noyau sur les systèmes UNIX. Elle signifie display message ou display driver. Elle récupère ses données en lisant le tampon d’anneau de noyau.
		- `who` affiche les informations de base telles que le nom d'utilisateur, le nom d'hôte, la date et l'heure de connexion.
		- `w` fournit des informations plus détaillées, telles que l'état du terminal, le temps d'activité et le processus en cours d'exécution.
		- `sudo blkid` ou `lsblk -fe7 -o +size` voir les disques, taille et FS
	- ### Commandes réseau
		- `ss -lptun` informations sur les ports ouverts
		- `lsof -Pi`afficher les fichiers ouverts qui sont des connexions réseau actives
		- `lsof -p <procNb>`afficher tous les fichiers ouverts concernant un processus
		- `bmon` ou `iptraf` voir le trafique en temps réel:
		- `tcpdump` et `wireshark` pour capturer et d’analyser le trafic
	- ## Canaux linux
		- `stdin` standard input
		- `stdout` standard output
		- `stderr` standard error
		- ![Capture d’écran du 2023-01-21 12-02-18.png](../assets/Capture_d’écran_du_2023-01-21_12-02-18_1674298954274_0.png)
		- Le caractère `<` permet de rediriger l'entrée standard (`stdin`) d'une commande à partir d'un fichier. Par exemple, la commande `sort < fichier.txt` utilise le contenu d'un fichier nommé `fichier.txt` comme entrée standard pour la commande `sort`.
		- `&` represente `stdin` et `stdout`, on peut rediriger `1` et `2` en faisant `&>`
		- Explication `> /dev/null 2>&1` (peut être remplacer par `&>/dev/null` ?) :
			- https://www.security-helpzone.com/2019/09/22/pourquoi-utiliser-dev-null-2-1/
	-
	- ## Variables d'environnement
		- Pour lister les variables d'environnement :
			- `env` ou `printenv`
		- Prompt : `$PS1`
		- Shell : `$SHELL`
	-
	- ## Arborescence
		- La fondation Linux est l'organisme responsable du maintien de la norme définissant l'arborescence des systèmes Unix/Linux. Cette norme est appelée FHS pour Filesystem Hierarchy Standard et est disponible sous plusieurs formats.
		- Selon FHS, voici la liste minimale de tous les éléments devant être présents
		  directement sous la racine / :
			- `bin` : répertoire contenant les commandes pouvant être utilisées à la fois par le système et par un administrateur ;
			- `boot` : répertoire contenant tout le nécessaire pour démarrer le système :
			  la configuration du bootloader, le noyau, les fichiers ramfs... ;
			- `dev` : répertoire contenant traditionnellement tous les points d'accès aux
			  périphériques, terminaux, disques, supports amovibles etc. ;
			- `etc` : répertoire contenant les fichiers de configuration des programmes
			  et services exploités sur le système ;
				- `/etc/os-release` contient les informations sur la distribution Linux
				- `/etc/pam.d/login` permet de gérer le processus d'authentification sous Linux
				- `/etc/shadow` contient les mots de passe des utilisateurs (cryptés)
			- `lib` : répertoire contenant les librairies partagées par les différentes
			  commandes de `/bin` ou `/sbin` ;
			- `media` : répertoire contenant les points de montage des périphériques
			  amovibles (clé USB, disques externes, etc.) ;
			- `mnt` : répertoire contenant les points de montage temporaires de
			  systèmes de fichiers requis à un instant donné par l'administrateur ;
			- `opt` : répertoire contenant les applications installées par-dessus le
			  système d'exploitation minimal ;
			- `run` : répertoire contenant les données gérées par le système depuis le
			  démarrage, ce répertoire est ré-initialisé entre chaque démarrage ;
			- `sbin` : répertoire contenant les commandes utilisées uniquement par
			  l'administrateur (pas par le système) ;
			- `srv` : répertoire contenant les données gérées par les services exploités
			  sur le système ;
			- `tmp` : répertoire servant de dépôt de fichiers temporaires pour le système,
			  les utilisateurs et les administrateurs ;
			- `usr` : répertoire complexe détaillé un peu plus loin dans le chapitre
			  “Adoptez l’arborescence des systèmes Linux” ;
			- `var` : répertoire contenant toutes les données variables du système,
			  qu'elles soient produites par les utilisateurs, les administrateurs ou le
			  système lui-même (comme les fichiers de traces).
		- Autres répertoires communs :
			- `/home` : créé pour stocker “les données personnelles de l'administrateur”, mais il n'est pas indispensable pour faire démarrer le système et l'exploiter. Et stocker dans ce répertoire des données critiques pour le système est effectivement une erreur.
			- `/home` : répertoire utilisateur
			- `/usr` : Ce répertoire est devenu avec le temps une arborescence majeure des distributions Linux. Il contient lui-même des répertoires `bin` et `sbin` dans lesquels se situent également des commandes !
			- `/var` : pour stocker toutes les informations utilisateurs, administrateurs et systèmes variables.
		- Système de fichiers virtuels, il n'est pas présent sur le disque dur mais maintenue en temps réel par le noyau Linux :
			- `/proc` : contient toutes les informations concernant le processus.
				- `/proc/cpuinfo` contient les informations sur le(s) processeur(s) maintenues par le noyau.
				- `/proc/version` contient la version exacte du noyau en exécution,
				- `/proc/meminfo`, les informations détaillés sur la mémoire vive gérée par le noyau,
				- `/proc/uptime`, le temps d'exécution cumulé,
				- `/proc/cmdline`, les paramètres passés au démarrage du noyau, etc.
			- `/sys` contient les informations sur les périphériques gérés par le noyau, notamment :
				- les périphériques de type bloc ou caractères dans les répertoires `/sys/block` ou `/sys/dev`,
				- les drivers dans `/sys/devices`,
				- les différents systèmes de fichiers dans `/sys/fs`,
				- les modules du noyau dans `/sys/module`.
				- `/sys/kernel` contient une arborescence de fichiers représentant des variables du noyau accessibles en écriture et permettant de modifier le comportement à chaud du système.
					- `/sys/kernel/debug` contient des fichiers permettant d'activer des fonctions de traces et de débogage du noyau.
	- ## Processus
		- `pstree` : processus en cours d'exécution sous forme d'arbre hierarchique. Voir aussi `ps -edf --forest`
		- `ps -edf` : voir tous les processus du système en utilisant la syntaxe traditionnelle.
		- `ps -edf --sort=+pmem | tail -5` : voir les 5 processus qui utilise le plus de mémoire.
		- `ps -aux'
		- `top` : display Linux processes. Taper `h` pour voir les commandes interactives.
		- `htop` : afficher les processus de manière plus graphique.
		- `nohup` permet d'exécuter une commande de manière à ne pas être affectée par les coupures de connexion (hangups), tout en redirigeant la sortie vers un périphérique qui n'est pas un terminal (non-tty).
		- `jobs` est utilisée pour afficher les processus en arrière-plan associés à votre session de terminal.
		- `fg %<idJob>` pour mettre au premier plan
		- `bg %<idJob>` pour mettre en arrière plan (mettre d'abord en pause avec `ctrl + z` pour pouvoir taper la commande)
		- `kill -l`pour voir les signaux existants
	- ### Priorité des processus
		- Le noyau gère l'allocation du temps CPU pour chaque processus en fonction d'un facteur de priorité. Le champ `NI` correspond à l'affichage de la valeur de ce facteur de priorité.
		- `renice +10 <processusNb>` Augmenter le facteur de priorité de 10. Plus la valeur de la priorité est élevée, moins le processus est prioritaire.
		- On peut aussi modifier la priorité dans `top` en tapant `r`
	- ### Tuer un processus
		- Pour tuer un processus, il faut lui envoyer un signal :
			- `SIGINT` (signal numéro 2) C'est le signal envoyé par `ctrl + c` ou `kill -2 <procNb>`
			- `SIGTERM` (signal numéro 15)
			- `SIGKILL` (signal numéro 9) Le signal est envoyé directement à `init` ou `systemd` qui se charge de tuer le processus `kill -9 <procNb>`
			- `killall <name>` tuer des processus par leur nom
	- ### Mettre en pause
		- `SIGSTOP` (signal numéro 19) ou `ctrl + z`
		- `SIGCONT`(signal numéro 18) pour reprendre
		-
	- ## configuration des logs
		- `/etc/rsyslog.conf`
	- ## En cas de freeze
		- Effectuez les combinaisons de touches suivantes, dans l'ordre :
			- `Alt SysRq s` - Synchronise les disques
			  `Alt SysRq e` - Essaie de fermer les processus en envoyant SIGTERM [facultatif]
			  `Alt SysRq i` - Tue tous les processus restant en envoyant SIGKILL [facultatif]
			  `Alt SysRq u` - Démonte les disques
			  `Alt SysRq b` - Redémarre
	- ## Sources
	- https://openclassrooms.com/fr/courses/7274161-administrez-un-systeme-linux