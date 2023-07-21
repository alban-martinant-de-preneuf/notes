- # Certificate Authority (CA)
	- ## Installation
		- Installer easy-rsa :
			- `sudo apt update`
			- `sudo apt install easy-rsa`
	- ## Préparer le répertoire PKI (Public Key Infrastructure)
		- Il faut être connecté avec un utilisateur normal.
		- On crée un dossier dans le home de l'utilisateur courant :
			- `mkdir ~/easy-rsa`
		- Puis on crée un lien vers les fichiers du package easy-rsa :
			- `ln -s /usr/share/easy-rsa/* ~/easy-rsa/`
			- (Dans la plupart des tutos, on fait une simple copie des fichier mais ici on fait un lien)
		- On restreint l'accès à l'utilisateur propriétaire :
			- `chmod 700 ~/easy-rsa`
		- On se place dans le répertoire et on initialise la PKI (Public key infrastructure) :
			- `cd ~/easy-rsa`
			- `./easyrsa init-pki`
	- ## Créer un certificat d'autorité
		- On crée dans le dossier "easy-rsa" un fichier nommé "vars" et on le rempli. On va simplement copier le fichier d'exemple déjà présent et le remplir :
			- `cp var.exemple vars`
			- `nano vars`
				- `set_var EASYRSA_REQ_COUNTRY    "FR"`
				  `set_var EASYRSA_REQ_CITY       "Marseille"`
				  `set_var EASYRSA_REQ_ORG        "MonOrganisation"`
				  `set_var EASYRSA_REQ_EMAIL      "alban.martinant-de-preneuf@laplateforme.io"`
				  `set_var EASYRSA_ALGO        "ec"`
				  `set_var EASYRSA_DIGEST         "sha512"`
		- Pour créer la paire de clés publique et privée racine de l'autorité de certification, exécuter à nouveau la commande ./easy-rsa, cette fois avec l'option build-ca :
			- `./easyrsa build-ca`
			- Choisir le mot de passe (`pass` dans mon cas)de la clé CA (Pour ne pas mettre de mot de passe, on peut ajouter l'option `nopass` à la commande précédente).
			- Cette commande crée `~/easy-rsa/pki/ca.crt` et `~/easy-rsa/pki/private/ca.key` qui sont les composants public et privé d'une autorité de certification.
				- `ca.crt` est le certificat public utilisé pour vérifier l'identité de l'autorité de certification et s'assurer que toutes les parties font partie du même réseau de confiance.
				- `ca.key` est la clé privée utilisée pour signer les certificats des serveurs et des clients, il est important de la protéger pour éviter les certificats frauduleux.
	- ## Distribuer le certificat public
		- On peut utiliser "ssh" avec "scp" pour récupérer ou envoyer le certificat. Pour le récupérer :
			- `scp user1@192.168.1.100:/home/user1/easy-rsa/pki/ca.crt /tmp/ca.crt`
		- Ensuite, on importe le certificat dans le magasin de certificats de son système d'exploitation :
			- `sudo cp /tmp/ca.crt /usr/local/share/ca-certificates`
			- `sudo update-ca-certificates`
		- Maintenant le système va faire confiance aux certificats signées par ce serveur CA.
	- ## Sources
		- https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04