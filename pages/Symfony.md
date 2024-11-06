- ## installation
	- Il faut que php et composer soient installés
	- Installer symfony-CLI
		- `curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash`
		- `sudo apt install symfony-cli`
	- Installation du squelette "Website Skeleton"
		- `symfony new --full mon-super-projet`
	- Nouveau projet (  )
		- `symfony new <projectName> --version=5.1`
	-
-
- ## démarrage serveur interne
	- `cd mon-super-projet`
	- Activer TLS :
		- `symfony server:ca:install`
	- `symfony server:start`
	- Ajouter l'option `-d` pour le lancer en détaché.
	- Pour les logs `symfony server:log`
	- Si le port n'est pas occupé, l'application sera alors disponible à cette adresse :  `http://localhost:8000/`
-
- ## Activer les abréviations emmet dans les .twig
	- Ajouter dans  `~/.config/Code/User/settings.json`
	- `"emmet.includeLanguages": {"twig": "html"}`
-
- ## Classes et methodes utiles
	- Pour créer une nouvelle requête avec la valeur des super globales
	  ```php
	  // Ou en paramètre de la fonction : public function test(Request $request)
	  $request =Request::createFromGlobals();
	  // Par exemple pour récupérer la valeur de $_GET["page"]
	  $request->query->get('page', 1) // 1 si il n'est pas set
	  ```
	- Paramètres
	  ```php
	  // src/Controller/BlogController.php
	  namespace App\Controller;
	  
	  use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
	  use Symfony\Component\HttpFoundation\Response;
	  use Symfony\Component\Routing\Annotation\Route;
	  
	  class BlogController extends AbstractController
	  {
	      // ...
	  
	      #[Route('/blog/{slug<\d+>?1}', name: 'blog_show')]
	      // <\d+> est le requierement pour dire que ça doit être un nombre
	      // ?1 pour dire que la valeur par défaut est 1
	      public function show(string $slug): Response
	      // On peut aussi récupérer la requête pour avoir accès à tout
	      // public function show(Request $request)
	      //    $request->attributes->get('slug')
	      {
	          // $slug will equal the dynamic part of the URL
	          // e.g. at /blog/yay-routing, then $slug='yay-routing'
	  
	          // ...
	      }
	  }
	  ```
-
- ## Cli
- infos
- `symfony console about`
- ## Sources
	- https://openclassrooms.com/fr/courses/5489656-construisez-un-site-web-a-l-aide-du-framework-symfony-5/7171301-installez-symfony-5
	- https://symfony.com/download
	- https://www.youtube.com/watch?v=4t3fNkGwRWo&t=2459s
	- https://symfony.com/doc/current/routing.html