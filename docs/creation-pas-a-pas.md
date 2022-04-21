# Création d'un site pas à pas à partir d'une maquette html/css

La création d'un nouveau site commence par duplication d'un site récent, je conseille ec1/03actionV2

Comme dis dans l'introduction, la première étape est d'attribuer l'identifiant du site, [cliquez ici](../#indexphp-à-modifier-avant-toute-chose-lorsqu39on-copie-un-site) pour la marche à suivre.

## 1. Le fichier skin.php
> se trouve dans /skins/index/skin.php

C'est ce fichier qui contient la structure globale du site. On y trouve l'en-tête, le header, menu, footer etc... En un mot tout ce qui n'est pas le contenu des pages web.

Voilà un exemple du minimum en terme de PHP que doit contenir le fichier afin de faire tourner le site :

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

		<?php if(!empty(eCat::text(eSystem::config('cat'),'name'))) eBlock::show('breadcrumps'); ?> 
	</div>
</div> <!--page-top-->
```

