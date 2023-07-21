- ## Installer et configurer
	- Installer
		- `apt-get install proftpd-*`
	- Créer un groupe
		- `addgroup ftpgroup`
	- Créer un utilisateur FTP
		- `adduser ftpuser -shell /bin/false -home /ftpshare`
	- Ajouter l'utilisateur au groupe
		- `adduser ftpuser ftpgroup`
	- Appliquer les permission sur le dossier de partage
		- `chmod -R 1777 /ftpshare/`
	- Créer un fichier de configuration personnalisé
		- `vi /etc/proftpd/conf.d/perso.conf`
		- `ServerName                      "Debian" # la bannière qui apparaît à la connexion`
		  
		  `UseIPv6 off # Pas de connexion IPv6`
		  `RootLogin       off # Interdire le login en root`
		  `RequireValidShell off # Pas besoin d'un shell valide (pour /bin/false)`
		- `#Le port 21 est le port FTP standard.`
		  `Port                            21`
		- `#pour restreindre l'accès des utilisateurs à leurs dossiers de départ uniquement`
		  `DefaultRoot  ~`
		- `# interdire les connexions hors du groupe ftpgroup ... si vous devez autoriser par exemple www-data, ne pas mettre ou ajoutez ce dernier dans le groupe FTP.`
		  `<Limit LOGIN>`
		    `DenyGroup !ftpgroup`
		  `</Limit>`
		  
		  `# definition du nombre de connexions max par clients, etc.`
		  `<IfModule mod_ctrls.c>`
		  `ControlsEngine        off`
		  `ControlsMaxClients    2`
		  `ControlsLog           /var/log/proftpd/controls.log`
		  `ControlsInterval      5`
		  `ControlsSocket        /var/run/proftpd/proftpd.sock`
		  `</IfModule>`
- ## Activer le TLS
	- `mkdir /etc/proftpd/ssl`
	- Générer certificat autosigné
		- `openssl req -new -x509 -keyout /etc/proftpd/ssl/proftpd.key -days 365 -nodes -out /etc/proftpd/ssl/proftpd.cert`
	- chmod 600 /etc/proftpd/ssl/proftpd.*
	- Créer le fichier `/etc/proftpd/conf.d/tls.conf`
		- `<IfModule mod_tls.c>`
		  	`TLSEngine	on`
		  	`TLSLog		/var/log/proftpd/tls.log`
		  	`TLSProtocol	SSLv23`
		  	`TLSOptions	NoCertRequest AllowClientRenegotiations`
		  	`TLSRSACertificateFile		/etc/proftpd/ssl/proftpd.cert`
		  	`TLSRSACertificateKeyFile	/etc/proftpd/ssl/proftpd.key`
		  	`TLSVerifyClient	off`
		  	`TLSRequired	on`
		  	`RequireValidShell	no`
		  	`TLSOptions	NoSessionReuseRequired`
		  `</IfModule>`
	- Modifier /etc/proftpd/modules.conf, décommenter
		- `LoadModule mod_tls.c`
	- `service proftpd restart`
-
- ## Sources
	- https://www.malekal.com/proftpd-configurer-un-serveur-ftp-sur-debian-10/
	- https://github.com/alban-martinant-de-preneuf/FTP