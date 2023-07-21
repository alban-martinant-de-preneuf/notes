- ## VoIP (Voice Over Internet Protocol)
	- La VoIP, encore appelée Voix sur IP ou Voice over Internet Protocol, est un système permettant de **transformer les signaux audio analogiques en données numériques** pouvant ainsi être transmises par Internet.
	- Quelques-unes des fonctionnalités de la VoIP :
		- Renvoi d’appel : Lorsqu’un poste ne répond pas, vous pouvez rediriger vos appels sur un téléphone fixe ou mobile, d’où que vous soyez.
		- Musique d’attente : Vous pouvez choisir une musique de fond durant les transferts d’appels.
		- Standard automatique personnalisé : Vous utilisez un seul et même répondeur pour l’entreprise et pouvez le personnaliser comme bon vous semble.
		- Appels internes gratuits : Les appels passés entre les différents postes d’une même entreprise sont gratuits.
		- Numéros abrégés : En interne vous pouvez utiliser des numéros abrégés de 4 chiffres ou paramétrer des numéros courts préenregistrés pour vos appels passés avec vos contacts réguliers.
		- Fax dématérialisé : Vous pouvez envoyer un fax depuis n’importe quel poste d’ordinateur grâce à une option imprimante virtuelle.
	- La VoIP est constituée d’une série de protocoles de télécommunications permettant de convertir et de transmettre des communications vocales sur un réseau de données grâce à la commutation de paquets.
-
- ## Installation
	- `apt update`
	- `apt install asterisk asterisk-dahdi`
	- Les fichiers de configuration sont dans `/etc/asterisk`
	- `cd /etc/asterisk`
	- Faire des backups des fichiers que l'on va modifier :
		- `cp sip.conf sip.conf.backup`
		- `cp extensions.conf extentions.conf.backup`
	- Supprimer toutes les lignes présentes
		- `echo | tee sip.conf`
		- `echo | tee extensions.conf`
	- Éditer `/etc/asterisk/sip.conf`
		- `[general]`
		  `context=default`
		  `allowoverlap=no`
		  `udpbindaddr=0.0.0.0`
		  `tcpenable=no`
		  `tcpbindaddr=0.0.0.0`
		  `transport=udp`
		  `srvlookup=yes`
		- `[9001]`
		  `type=friend`
		  `host=dynamic`
		  `secret=9001`
		- `[9002]`
		  `type=friend`
		  `host=dynamic`
		  `secret=9002`
	- Éditer `/etc/asterik/extensions.conf`
		- `[general]`
		  `static=yes`
		  `writeprotect=no`
		  `priorityjumping=no`
		  `autofallthrough=yes`
		  `clearglobalvars=no`
		- `[default]`
		  `exten => 9001,1,Dial(SIP/9001,10)`
		  `exten => 9002,1,Dial(SIP/9002,10)`
	- Redémarrer le service
		- `systemctl restart asterisk`
-
- ## Sources
	- https://www.napsis.fr/telephonie-ip/voip-voix-ip/