# Gestion des pages

## Code utile

#### Nom

```php
eArray::getVal($data,'text['.eSystem::config('lng').'][name]')
```

#### Description courte

```php
eArray::getVal($data,'text['.eSystem::config('lng').'][short]')
```

#### Description 1

```php
eArray::getVal($data,'text['.eSystem::config('lng').'][text1]')
```

#### Description 2

```php
eArray::getVal($data,'text['.eSystem::config('lng').'][text2]')
```

#### Code

```php
eArray::getVal($data,'code')
```

#### NoTitle

> Pour ne pas afficher le titre de la page sur le site public

```php
eArray::getVal($data,'data[notitle]')
```

```php
//cas d'utilisation
if(empty(eArray::getVal($data,'data[notitle]'))) { echo '<h2>'.eArray::getVal($data,'text['.eSystem::config('lng').'][name]').'</h2>'; }
```

#### Option

```php
eArray::getVal($data,'data[options]')
```

#### Tag

```php
eArray::getVal($data,'data[tag1]')
```

## Génération des images

#### Affichage de plusieurs images (type galerie)

```php
<?php 
    $array_img = ePages::getImgArray($data,'img1');	
    if(!empty($array_img))foreach($array_img as $key => $val){
        $tag1 = $tag2 = $tag3 = '';
        if(!empty($data['text'][eSystem::config('lng')]['img1'][$val['id2']]['tag1'])) $tag1 = $data['text'][eSystem::config('lng')]['img1'][$val['id2']]['tag1'];
        if(!empty($data['text'][eSystem::config('lng')]['img1'][$val['id2']]['tag2'])) $tag2 = $data['text'][eSystem::config('lng')]['img1'][$val['id2']]['tag2'];
        if(!empty($data['text'][eSystem::config('lng')]['img1'][$val['id2']]['tag3'])) $tag3 = $data['text'][eSystem::config('lng')]['img1'][$val['id2']]['tag3'];
            //tout ce qui se trouve dans le echo sera généré pour chaque image
            echo '<a href="'.$val['file'].'" data-fancybox="gallery"><img src="'.eImages::imgCache($val['file'],'x400').'" /></a>';
    }
?>
```

#### Affichage d'une seule image

```php
<?php 

$image = array(); // declaration du tableau qui contient les iamges

//creation de la boucle qui va remplir le tableau
$array_img = ePages::getImgArray($data,'img1');	
if(!empty($array_img))foreach($array_img as $key => $val){
    $image[] = $val['file'];
}

//affichage d'une seule image (ici la première image dans une balise <img>)
echo '<img src="'.$image[0].'" alt=""/>'; 

//affichage d'une seule image (ici la deuxième image dans en tant que background d'une div)
echo '<div style="background:url('.$image[1].') no-repeat center;"></div>'; 

?>
```