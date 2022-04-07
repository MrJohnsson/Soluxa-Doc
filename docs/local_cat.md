# Gestion des catégories

## Code utile

#### Nom 1

```php
eCat::text(eSystem::config('cat'),'name')
```

#### Nom 2

```php
eCat::text(eSystem::config('cat'),'title')
```

#### Description de la catégorie

```php
eCat::$values['cat'][eSystem::config('cat')]['text'][eSystem::config('lng')]['desc']
```

#### Code

```php
eCat::data(eSystem::config('cat'),'code')
```

#### Tag

```php
eCat::data(eSystem::config('cat'),'tag1')
```

#### Options

```php
eCat::data(eSystem::config('cat'),'options')
```

#### IMG 1

```php
eCat::data(eSystem::config('cat'),'image1')
```

#### IMG 2

```php
eCat::data(eSystem::config('cat'),'image2')
```

