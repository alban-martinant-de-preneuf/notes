- ## Pour le `.env`
- Ont met la clé secrète dans le `.env`
- ```text
  JWT_SECRET=secret_key
  ```
- ```bash
  composer require vlucas/phpdotenv
  ```
- ```php
  require 'vendor/autoload.php';
  $dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
  $dotenv->load();
  ```
- Les variables sont ensuite accessibles dans $_ENV
-
- ## Générer le token
- ```bash
  composer require firebase/php-jwt
  ```
- ```php
  require 'vendor/autoload.php';
  
  use Firebase\JWT\JWT;
  
  $payload = [
          'iat' => time(),
          'exp' => time() + 60 * 60,
          'id' => $user['id'],
          'email' => $user['email']
      ];
  
      $key = $_ENV['JWT_KEY'];
      $jwt = JWT::encode($payload, $key, 'HS256');
  ```