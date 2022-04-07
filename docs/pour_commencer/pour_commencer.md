# Fonctionnement nouveau CMS

## Architecture d'un site

![architecture](/media/architecture.png)

### Index.php, à modifier avant toute chose lorsqu'on copie un site

> Ce fichier permet de lier le projet à sa base de donnée correspondante, nommée par le même identifiant que celui du projet.

> :warning: Si on ne le modifie pas les données du site copié seront écrasées


    <?php
    /*==================================================================*/
    /*  CMS SOLUXA																										  */
    /*  www.soluxa.com            	                          				  */
    /*==================================================================*/
    /*                              	                                  */
    /*  START CMS                                                     	*/
    /*  					                                                    	*/
    /*==================================================================*/

        //define('SRC_ID', 'src1'); //eg, "demo"		
        $GLOBALS['slx_config']['src_id'] = '000demo'; // l'identifiant est à mettre à la place de '000demo'
        
        require_once('../../_slx/start.php');
        
    ?>


### Skins

Le thème du site avec le css et le fichier de structure en php (skin.php)

### _local

Toutes les vues du site séparées en sous-catégories :

#### blocks

correspond aux widgets sur wordpress ; des morceaux de contenu hors pages (comme une adresse dans le footer par exemple)

#### cat

le conteneur d’une page de contenu. Il y en a peu, en général on a une vue pour les pages de contenu normal et une pour les actualités.

#### pages

les vues pour chaque type de présentation de contenu. Elles permettent d’éviter tout code dans l’éditeur et de manier le contenu très facilement par le client dans l’admin 
le conteneur d’une page de contenu. Il y en a peu, en général on a une vue pour les pages de contenu normal et une pour les actualités.

#### shop

l’ensemble des vues concernant la boutique en ligne. Cela concerne les vues des produits, listes de produits, panier, mon compte etc..