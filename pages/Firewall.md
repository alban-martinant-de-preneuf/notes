- !! Attention de  ne pas se bloquer quand on est en ssh !!
- # UFW
- UFW est une surcouche de iptables
- `sudo ufw status` (on peut ajouter `verbose`)
- La configuration se trouve dans `/etc/default/ufw`
- exemple de configuration :
	- `sudo ufw default deny incoming`
	- `sudo ufw default allow outgoing`
	- `sudo ufw allow https` (ou 443. Se base sur `/etc/services`)
	- Par défaut s'applique aux communications entrantes, c'est comme faire :
	- `sudo ufw allow in https` (Pour les communications sortante `out`)
- remettre à 0 :
	- `sudo ufw reset`
-