<p align="center"><img src="art/lartisan_logo.png" width="300" height="45"></p>

# Changes to the original package

This forked package eliminates the **IsSorted** Trait from `Spatie\MediaLibrary\MediaCollections\Models\Media`, so the sorting/ordering part could be made by [spatie/eloquent-sortable](https://github.com/spatie/eloquent-sortable).
I have also added the `spatie/eloquent-sortable` as a dependency.

## Installation

This package uses `"spatie/eloquent-sortable": "^3.11"` together with `"spatie/laravel-medialibrary": "^9.0.0"` and it can be installed through Composer:
```bash
composer require lartisan/laravel-sortable-medialibrary
```
In Laravel 5.5 and above the service provider will automatically get registered. In older versions of the framework just add the service provider in config/app.php file:
```php
'providers' => [
    ...
    Spatie\MediaLibrary\MediaLibraryServiceProvider::class,
    Spatie\EloquentSortable\EloquentSortableServiceProvider::class,
];
```
Optionally you can publish the config files with:
```bash
php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="migrations"
php artisan vendor:publish --provider="Spatie\EloquentSortable\EloquentSortableServiceProvider" --tag="config"
```

## Usage

#### To add the intended Eloquent sortable behaviour to your custom Media model you must:
1. Implement the `Spatie\EloquentSortable\Sortable` interface.
2. Use the trait `Spatie\EloquentSortable\SortableTrait`.
3. Optionally specify which column will be used as the order column. The default is `order_column`.
4. Optionally, if your model/table has a grouping field, you can create a `buildSortQuery` method at your model.

Example:
```php
...
use Spatie\EloquentSortable\Sortable;
use Spatie\EloquentSortable\SortableTrait;
use Spatie\MediaLibrary\MediaCollections\Models\Media as BaseMedia;

class MyCustomMedia extends BaseMedia implements Sortable
{
    use SortableTrait;
    
    /**
     * Define the column used for sorting
     * @var array
     */
    public $sortable = [
        'order_column_name' => 'order_column',
        'sort_when_creating' => true,
    ];

    /**
     * @return Builder
     */
    public function buildSortQuery(): Builder
    {
        return static::query()
            ->where('collection_name', $this->collection_name);
    }
    ...
}
```

#### To add the default sort of the original package behaviour to your custom Media model

You could still use the default **IsSorted** behaviour of the original package by adding the `Spatie\MediaLibrary\MediaCollections\Models\Concerns\IsSorted` to your model.

Example:
```php
...
use Spatie\MediaLibrary\MediaCollections\Models\Concerns\IsSorted;
use Spatie\MediaLibrary\MediaCollections\Models\Media as BaseMedia;

class MyCustomMedia extends BaseMedia
{
    use IsSorted;
}
```

## Documentation

The original [spatie/laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/v8) package.

The [spatie/eloquent-sortable](https://github.com/spatie/eloquent-sortable) package.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
