# Création d'un site pas à pas à partir d'une maquette html/css

La création d'un nouveau site commence par duplication d'un site récent, je conseille ec1/03actionV2

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

### Afficher un contenu uniquement en accueil mais différent sur les autres pages :

```php
<?php 

if(eSystem::isHome()){
    // ici on place le contenu qui sera uniquement en accueil
}else{
    // ici on place le contenu qui sera sur les autres pages
}

?>
```
### Afficher un page-top (une bannière qui contient le nom de la catégorie et une image en fond)

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

## Afficher du contenu pour certaines pages générées automatiquement

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