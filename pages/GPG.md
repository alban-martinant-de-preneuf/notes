- # Générer la clé
	- Pour la générer de manière intéractive :
		- `gpg --full-gen-key`
-
- ## Configurer l'agent de cache de mot de passe
- Curieusement, l’outil GPG met en cache les mots de passe. De ce fait, vous (ou toute personne ayant accès à votre système) pourriez décrypter le fichier sans avoir à taper le mot de passe avec la commande gpg zdnet_test. Ce n’est pas sûr. Pour contourner ce problème, nous devons désactiver la mise en cache du mot de passe pour l’agent GPG. Pour ce faire, créez un nouveau fichier avec la commande :
- `nano ~/.gnupg/gpg-agent.conf`
- Dans ce fichier, collez les lignes suivantes :
- `default-cache-ttl 1 max-cache-ttl 1`
- Ensuite, redémarrez l’agent avec la commande :
- `echo RELOADAGENT | gpg-connect-agent`
- Maintenant, lorsque vous (ou n’importe qui) tapez la commande de décryptage, `gpg zdnet_test`, l’invite du mot de passe apparaîtra. Jusqu’à ce que le mot de passe soit saisi avec succès, le contenu du fichier restera crypté.
-
- ## Encrypter avec la clé généré
	- `gpg --encrypt --recipient "NomDeLaCle" --output encryptedfile.gpg plaintextfile.txt`
	- supprimer le fichier non crypté
		- `rm -i plaintextfile.txt`
-
- ## Décrypter avec la clé privée
	- `gpg --decrypt encryptedfile.gpg > decryptedfile`
-
-
- ## Encypter avec une clé symétrique
	- `gpg --cipher-algo AES256 -c file.txt`
	- Ceci génère un fichier crypté avec l'extension .gpg
	- supprimer les fichier non chiffré
		- `rm -i file.txt'
	-
-
-
- ## Méthode GUI
	- une fois la clé généré :
		- `sudo apt-get install seahorse-nautilus -y`
-
- ## sources
- https://www.zdnet.fr/pratique/comment-chiffrer-un-fichier-sous-linux-39956852.htm