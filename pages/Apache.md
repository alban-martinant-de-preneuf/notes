- ## Installation
	- `apt update`
	- `apt install apache2 openssl`
-
- ## Activer HTTPS
	- `apt install openssl`
	- Activer les modules apache `Mod_ssl` et `Mod_rewrite`
		- `a2enmod ssl`
		- `a2enmod rewrite`
	- Modifier `/etc/apache2/apache2.conf`
		- `<Directory /var/www/html>`
		  `AllowOverride All`
		  `</Directory>`
	- Créer une clé privée et le certificat de site Web avec OpenSSL
		- `mkdir /etc/apache2/certificate`
		  `cd /etc/apache2/certificate`
		  `openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out apache-certificate.crt -keyout apache.key`
	- Modifier `/etc/apache2/sites-enabled/000-default.conf`
		- `<VirtualHost *:80>`
		          `RewriteEngine On`
		          `RewriteCond %{HTTPS} !=on`
		          `RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R=301,L]`
		  `</virtualhost>`
		  `<VirtualHost *:443>`
		          `ServerAdmin webmaster@localhost`
		          `DocumentRoot /var/www/html`
		          `ErrorLog ${APACHE_LOG_DIR}/error.log`
		          `CustomLog ${APACHE_LOG_DIR}/access.log combined`
		          `SSLEngine on`
		          `SSLCertificateFile /etc/apache2/certificate/apache-certificate.crt`
		          `SSLCertificateKeyFile /etc/apache2/certificate/apache.key`
		  `</VirtualHost>`
	- `service apache2 restart`
- ## Sources
	- https://techexpert.tips/fr/apache-fr/activer-https-sur-apache/