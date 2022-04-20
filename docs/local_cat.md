# Gestion des catégories

## Code utile

```php
// Nom 1
eCat::text(eSystem::config('cat'),'name')
```


```php
// Nom 2
eCat::text(eSystem::config('cat'),'title')
```

```php
// Description de la catégorie
eCat::$values['cat'][eSystem::config('cat')]['text'][eSystem::config('lng')]['desc']
```

```php
// Code
eCat::data(eSystem::config('cat'),'code')
```

```php
// Tag
eCat::data(eSystem::config('cat'),'tag1')
```

```php
// Options
eCat::data(eSystem::config('cat'),'options')
```

```php
// Image 1
eCat::data(eSystem::config('cat'),'image1')
```

```php
// Image 2
eCat::data(eSystem::config('cat'),'image2')
```