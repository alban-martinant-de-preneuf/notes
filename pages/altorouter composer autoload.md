- # Composer altorouter et autoload
	- ## Installation et mise en route
		- `sudo a2enmod`
		- `sudo systemctl restart apache2`
		- Modifier le fichier de config `/etc/apache2/sites-available/000-default.conf`
		- `<Directory /var/www/html>`
		         `Options Indexes FollowSymLinks`
		         `AllowOverride All`
		         `Require all granted`
		   `</Directory>`
		- `sudo systemctl restart apache2`
		- (Composer dois être installé)
			- `curl -s https://getcomposer.org/installer | php`
			- `sudo mv composer.phar /usr/local/bin/composer`
		- `composer require altorouter/altorouter`
		- Ajouter `vendor/` à `gitignore`
		- Créer le fichier `.htaccess` à la racine du projet et l'éditer :
			- ```code
			  RewriteEngine on
			  RewriteCond %{REQUEST_FILENAME} !-f
			  RewriteRule . index.php [L]
			  ```
		- Créer le fichier `.index.php` à la racine du projet et l'éditer :
			- ```php
			  <?php
			  require_once 'vendor/autoload.php';
			  $router = new AltoRouter();
			  // (Il faut redémarrer vscode si il ne reconnait pas AltoRouter)
			  $router->setBasePath('/super-week');
			  $router->map('GET', '/', function () {
			      echo "OK";
			  }, 'home');
			  
			  // match current request url
			  $match = $router->match();
			  // call closure or throw 404 status`
			  if (is_array($match) && is_callable($match['target'])) {
			      call_user_func_array($match['target'], $match['params']);
			  } else {
			      // no route was matched
			      header($_SERVER["SERVER_PROTOCOL"] . ' 404 Not Found');
			  }
			  ```
		- ## autoload
			- Dans `composer.json` ajouter :
				- ```json
				  "autoload": {
				  	"psr-4": {
				      	"App\\": "src/"
				  	}
				  }
				  ```
			- `composer dump-autoload` ou `composer install` si composer.json vient d'être créé.
			- This command will re-generate the vendor/autoload.php file. See the dump-autoload section for more information.
			-
		-
		-