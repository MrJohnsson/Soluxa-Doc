# Création d'un site pas à pas à partir d'une maquette html/css

La création d'un nouveau site commence par duplication d'un site récent, je conseille ec1/03actionV2, c'est sur ce site que se base la démonstration.

Comme dis dans l'introduction, la première étape est d'attribuer l'identifiant du site, [cliquez ici](../#indexphp-à-modifier-avant-toute-chose-lorsqu39on-copie-un-site) pour la marche à suivre.

## 1. Le fichier skin.php
> se trouve dans /skins/index/skin.php

C'est ce fichier qui contient la structure globale du site. On y trouve l'en-tête, le header, menu, footer etc... En un mot tout ce qui n'est pas le contenu des pages web.

La première étape est donc de transformer votre fichier de maquette html en skin.php, ce qui consiste à remplacer tout le contenu par certaines instructions et fonctions PHP qui feront appel à la base de donnée afin d'obtenir un site dynamique.

Voilà un exemple du minimum en terme de PHP que doit contenir le fichier afin de pouvoir faire tourner le site :

```php
<!DOCTYPE HTML>
<html lang="fr">
<head>

<?php eSkin::show('meta'); //appel aux métadonnées qui sont générées automatiquement ou manuellement via l'admin ?>

<link rel="stylesheet" href="<?php eSkin::show('skin_dir');?>style.css" type="text/css" />

<script type="text/javascript" src="<?php eSkin::show('skin_dir');?>js/jquery-1.7.2.min.js"></script>

<meta name="viewport" content="width=device-width, user-scalable=no" />
<meta name="format-detection" content="telephone=no" />

</head>
<body>

    <header>
        <!-- LOGO "eLink::home();" ce code correspond au lien de la page d'accueil, peu importe où on se trouve sur le site-->
		<a href="<?php echo eLink::home();?>" class="logo"></a>        

        <!-- MENU PRINCIPAL -->
        <!-- le menu est généré à partir d'un autre fichier (/_local/blocks/default/menu.php), nous verrons plus loin de quelle façon et comment le personnaliser-->
		<?php eBlock::show('menu','file_local'); ?>	      
    </header>
    
    <div class="site-content">

        <!-- SLIDER ACCUEIL-->
        <?php if(eSystem::isHome()){ // cette condition détecte si on se trouve sur la page d'accueil (la page d'accueil est définie dans l'administration en cliquant sur l'icône "maison" à droite de la catégorie)
            eBlock::show('slider'); // grâce à la condition ci-dessus on peut afficher le slider uniquement sur la page d'accueil
            } // fin de la condition
        ?>
        
        <!-- CONTENU DU SITE -->
        <!-- c'est cette instruction PHP qui va afficher le contenu de la catégorie et des pages qu'elle contient -->
        <?php eSkin::show('content');?>

    </div>

    <footer>
        // Le footer est souvent créé dans un "block spécial" qu'on peut assimiler à un widget
        // Il s'agit de petits contenus indépendants de l'arborescence que l'on peut appeler à tout endroit
        <?php eBlock::show('footer'); ?>
    </footer>

</body>
</html>
```
Voilà donc le code très basique du skin.php
A présent à vous de l'agrémenter du contenu html supplémentaire, appel aux scripts, google fonts etc...

## Aller plus loin avec le skin.php

> Maintenant que la base a été vue, voyons quelques cas d'utilisations pratiques souvent rencontrés dans la création de sites qui peuvent être utiles :

### 1.1 Afficher un contenu uniquement en accueil mais différent sur les autres pages :

```php
<?php 

if(eSystem::isHome()){
    // ici on place le contenu qui sera uniquement en accueil
}else{
    // ici on place le contenu qui sera sur les autres pages
}

?>
```
### 1.2 Afficher un page-top (une bannière qui contient le nom de la catégorie et une image en fond)

> Code à placer dans skin.php
```php
<div <?php eBlock::show('cat_bg'); ?>>  <!-- bloc pour afficher l'image de la catégorie en page-top -->
	<div class="titre">
	<h1><?php 

		if(!empty(eCat::text(eSystem::config('cat'),'title'))){ //si le nom 2 de la cat n'est pas vide on l'affiche
			echo eCat::text(eSystem::config('cat'),'title'); 
		}else if(!empty(eCat::text(eSystem::config('cat'),'name'))){  // sinon on affiche le nom 1
			echo eCat::text(eSystem::config('cat'),'name'); 
		}else{ 
			echo eSystem::skin('meta_title'); // si ni l'un ni l'autre n'est renseigné on affiche le meta title
		} ?></h1>

		<?php eBlock::show('breadcrumps'); //affichage du fil d'ariane ?> 
	</div>
</div> <!--page-top-->
```

> Code du bloc cat_bg (qui se trouve dans local/blocks/default/cat_bg.php)
```php
<?php
	if(empty($type)) $type = 'image1';
	$image = eCat::data(eSystem::config('cat'),$type); // on regarde si l'image1 de la catégorie est renseignée
	if(empty($image)) {
		$content = '';

		$content = 'class="page-top no-img"';	// si il n'y a pas d'image voilà ce qu'on affiche

		echo $content;
		return false;
	}else{

	$content = 'class="page-top" style="background:url('.$image.') no-repeat center;background-size: cover;"'; //sinon on affiche l'image en background

	echo $content;

	}
?>
```

## 1.3 Afficher du contenu pour certaines pages générées automatiquement

> Il peut arriver que certaines pages précises nécessitent un conteneur mais on ne peut pas modifier leur vue.
> Pour cela on peut les détecter avec leur type et afficher du code spécifique

```php
if(eSystem::config('mod') == 'sitemap')  // plan du site
if(eSystem::config('mod') == 'search')   // résultat de la recherche
if(eSystem::config('mod') == 'facture')  // page spécifique pour payer une facture
```

# 2. Création de l'arborescence via l'administration

A ce stade là vous ne devriez pas voir grand chose car sur le site car la base de donnée est encore vide.<br>
Nous allons commencer à ajouter du contenu en commençant par l'arborescence.

Rendez-vous donc sur l'admin (en ajoutant /admin à l'url du site), vous arrivez sur l'arborescence.<br>
Cliquez sur "Ajoutez une nouvelle catégorie", vous obtenez la même chose que la capture ci-dessous :

![ajout d'une catégorie](/media/admin-cat.jpg)

> Voilà ce à quoi correspond chaque élément :
> 1. Accéder aux paramètres généraux des catégories
> 2. Ajouter une page à l'intérieur de la catégorie (pensez à d'abord créer la catégorie en cliquant sur enregistrer)
> 3. Permet de dupliquer une catégorie déjà créée (seule la catégorie est copiée, cela n'inclue pas les pages qu'elle contient)
> 4. Cette case définit la page d'accueil
> 5. Cette case permet d'activer ou désactiver la catégorie. Si elle est désactivée la catégorie n'apparaîtra pas dans le menu mais sera toujours accessible via le lien direct
> 6. Le code permet de créer une variante. Les différents codes sont ajoutés dans les paramètres généraux (1)
> 7. L'aperçu est ce qui définit quel type de contenu est affiché (par défaut "pages", sinon cela peut être "news" pour les actualités, "mod : shop" pour une catégorie de produits, "link :" permet de rediriger vers une autre page, interne ou externe)
> 8. Le tag peut être utilisé pour y insérer une information, on peut le récupérer pour appliquer une classe particulière à une div par ex.
> 9. L'image 1 correspond à ce qu'on utilise pour le page-top. Le menu déroulant affiche les images du dossier img/cat, c'est à configurer dans les paramètre généraux.
> 10. On peut avoir une deuxième image, par exemple pour une miniature lorsque on a un sous-menu avec des images
> 11. Options est une autre façon d'ajouter une information
> 12. Nom de la catégorie
> 13. Cette case permet de désactiver la catégorie uniquement dans la langue en cours d'édition.
> 14. Nom 2, il permet d'avoir un nom différent entre le menu et le nom que l'on affiche dans le contenu.
> 15. Description de la catégorie
> 16. Métadonnées
> 17. Enregistrer et rester sur la page
> 18. Enregistrer et retourner en arrière

Commencez par la page d'accueil en renseignant le nom (12) et en cliquant sur la case accueil (14).<br>
Cliquez sur enregistrer et retourner puis recliquez sur "Ajouter une nouvelle catégorie" afin de continuer votre arborescence.<br>
Afin de créer un sous-menu il suffit de cliquer sur le nom de la catégorie parente puis de cliquer sur "Ajouter une nouvelle catégorie"

:warning: Dans l'arborescence il y a 2 actions bien distinctes pour les catégories, si on clique sur l'icône du dossier on accède aux paramètres de la catégorie et si on clique sur le nom on accède à son contenu.

# 3. Génération du menu

Après que l'arborescence a été créée via l'admin, nous devrions voir apparaître le menu sur le site.
Ce menu est généré à partir du fichier _local > blocks > default > menu.php

Il y a 3 endroits dans ce fichier que vous pourrez être amenés à modifier :

### 3.1 Item avec sous-menu

```php
// QUAND IL Y A UN SOUS-MENU
$content.= "\n".'<li   '.(!empty($active)?' class="selected has-sub"':'').' class="has-sub">
					<a href="'.$link.'" class="'.(!empty($active)?' active':'').'" data-toggle="dropdown">
						<span>'.eCat::name($val['id']).'</span>
					</a>
					<div class="dropdown-wrap">
						<div class="dropdown-triangle"></div>
					</div>';      
$content.= "\n".'<ul class="sous-menu">'.$submenu."\n".'</ul>';      
$content.= "\n".'</li>';  
```

Concerne tous les éléments qui contiennent un sous-menu. En général on a besoin que de 4 variables :
- **$active** : détecte quand on est dans une catégorie pour lui assigner une class "selected" par exemple
- **$link** : l'url de la catégorie
- **eCat::name($val['id'])** : le nom de la catégorie
- **$submenu** : le sous-menu

### 3.2 Item sans sous-menu

```php
// QUAND IL N'Y EN A PAS (DERNIER NIVEAU)
$content.= "\n".'<li '.(!empty($active)?' class="selected"':'').'>
					<a href="'.$link.'">
					<span>'.eCat::name($val['id']).'</span>
					</a>
				</li>';  
```

Concerne les éléments de dernier niveau qui ne contiennent donc pas de sous-menu.<br>
Cela peut parfois prêter à confusion, voilà un exemple d'arborescence afin d'expliquer ce qu'est considéré un sous-menu et un dernier niveau :

> - Accueil *dernier niveau*
> - Le Domaine *sous-menu*
> 	- Notre histoire *dernier niveau*
> 	- Le Vignoble *dernier niveau*
> 	- La Famille *dernier niveau*
> - Nos vins *sous-menu*
> 	- Notre philosophie *dernier niveau*
> 	- Nos différentes gammes *sous-menu*
> 		- Tradition *dernier niveau*
> 		- Grands Crus *dernier niveau*
> 		- Crémants *dernier niveau*
> 	- Commande rapide *dernier niveau*
> - Galerie *dernier niveau*
> - Contact *dernier niveau*

### 3.3 Affichage du menu

Une fois que le menu est généré, il faut l'afficher. C'est là que ce code intervient :

```php
<!-- AFFICHAGE DU MENU -->

<div class="main-menu">
  <ul>
    <?php echo slx_menu(eCat::$tree);?> 
  </ul>
</div> <!-- main-menu -->
```

### 3.4 Variables supplémentaires pour la génération du menu

Voilà quelques variables afin d'afficher plus d'éléments dans le menu :

- **eCat::data($cat,'image1')** : Afficher l'image1 de la catégorie
- **eCat::data($cat,'image2')** : Afficher l'image2 de la catégorie
- **eCat::text($val['id'],'title')** :: Nom 2 de la catégorie
- **eCat::text($val['id'],'desc')** :: Description de la catégorie

# 4. Page d'accueil

A présent nous pouvons attaquer le contenu. Commençons par la page d'accueil.<br>
Pour rappel, voilà comment afficher du contenu uniquement en accueil :
```php
<?php 

if(eSystem::isHome()){
    // ici on place le contenu qui sera uniquement en accueil
}

?>
```

### 4.1 Slider

> Le slider est créé à partir d'une page et d'un bloc

On commence par créer une page "Slider" dans la catégorie Accueil. On lui donne pour code "slider" et on désactive la page (cela permet de pouvoir l'afficher en l'appelant dans le bloc slider.php).<br>
Avant de pouvoir ajouter des images il faut activer le champ correspondant. Cliquer sur l'icône Paramètres en haut à droite et cochez les éléments suivants :<br>
![parametres page](/media/parametres-page.jpg)

A présent vous pouvez revenir sur la page Slider et ajouter vos images en cliquant sur "Sélect fichiers".

> Lorsqu'on upload à partir d'une page elle est uploadée dans le dossier img > pages et est renommée "3-img1-2.jpg", ce qui correspond respectivement à l'ID de la page (ici 3), le type d'image (img1, on peut avoir un deuxième champ pour importer les images si besoin : img2) et le numéro de l'image (ici 2).<br>
> :warning: Attention si vous renommez une photo après l'avoir uploadée elle ne sera plus liée à la page.

A présent que la page est créée et les images uploadées, la suite se passe dans le fichier _local > blocks > default > slider.php

La librairie utilisée la plupart du temps pour faire fonctionner le slider est slick.js mais il est possible d'en utiliser une autre bien sûr.<br>
Le principe est toujours le même :
- Une boucle qui va générer du code pour chaque image
- Du code html qui englobe la boucle
- Un script qui initialise le slider

Enfin pour ajouter le slider à la page d'accueil il suffit d'appeler le bloc :
```php
<?php eBlock::show('slider'); ?>
```

### 4.2 Blocs de contenu

> Il n'y en a pas dans le modèle 03actionV2 mais c'est beaucoup utilisé sur d'autres sites

![Blocs accueil](/media/blocs-accueil.jpg)

#### Partie admin

Afin de garantir une gestion facile dans l'admin, chaque bloc est séparé et mis dans un **block spécial**
> Pour rappel les blocks spéciaux sont des contenus hors de l'arborescence qui peuvent être appelés à tout endroit.
> Ils ont très pratiques pour afficher du contenu sans à avoir recours à des divs dans l'éditeur (on les mettra dans les fichiers php autour de l'appel au bloc)

Rendez vous donc dans les blocks spéciaux et cliquez sur "Ajouter un nouveau block spécial".<br>
Dans Code renseignez le nom afin d'être explicite. Attention ce code correspond à l'ID donc il ne faut pas mettre d'espace ni d'accent.<br>
Ici on met d'habitude "block-accueil1" pour le premier et ainsi de suite. Dans la description on va mettre le contenu du bloc.

#### Partie php

Une fois que les trois blocs sont créés nous allons les appeler sur le site. Voilà comment j'ai l'habitude de procéder :
- Rendez vous dans la catégorie accueil afin d'ajouter une page
- Mettez par exemple le code "blocs3" à cette page
- A présent ouvrez le fichier *_local > pages > view > index-pages.php*
- Ajoutez ce code en haut du fichier (le html est à personnaliser bien sûr) :
```php
<?php if(eArray::getVal($data,'code')=='blocs3'){ ?>
	<div class="container">
		<div class="blocs3_wrap">
			<div class="col-md-4 col-sm-12">
				<div class="bloc3 ">
				<div class="content">
					<?php eBlock::show('bloc-accueil1'); ?>
					</div>
				</div>
			</div>
			<div class="col-md-4 col-sm-12">
				<div class="bloc3 ">
				<div class="content">
					<?php eBlock::show('bloc-accueil2'); ?>
				</div>
				</div>
			</div>
			<div class="col-md-4 col-sm-12">
				<div class="bloc3 rose">
				<div class="content">
					<?php eBlock::show('bloc-accueil3'); ?>
				</div>
				</div>
			</div>
		</div>
	</div>

 <?php } ?>
```

(je vous renvoie vers la partie PAGES pour plus d'explications)