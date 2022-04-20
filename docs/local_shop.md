# Fiche produit

## Code utile

```php
// Nom 1
eArray::getVal($data,'text['.eSystem::config('lng').'][name]')
```

```php
// Nom 2
eArray::getVal($data,'text['.eSystem::config('lng').'][name2]')
```

```php
// Description courte
eArray::getVal($data,'text['.eSystem::config('lng').'][short]')
```

```php
// Description 1
eArray::getVal($data,'text['.eSystem::config('lng').'][text1]')
```

```php
// Référence
eArray::getVal($data,'model')
```

```php
// Prix
eShop::getFormatPrice($data['id'])
```

```php
// Conditionnement (à créer en amont dans l'admin : admin.php?mod=setup&i=prod-pack)
$pack[$data['data']['pack']]['desc']
```

```php
// Options
eArray::getVal($data,'data[options]')
```

```php
// Fichier (importer une fiche produit en pdf existante par ex.)
eArray::getVal($data,'images[file1][0][file]')
```