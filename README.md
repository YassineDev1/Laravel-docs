# Laravel-docs
### Blade directive to add true/false conditions

New in Laravel 8.51: `@class` Blade directive to add true/false conditions on whether some CSS class should be added. Read more in [docs](https://laravel.com/docs/8.x/blade#conditional-classes)

Before:

```php
<div class="@if ($active) underline @endif">`
```

Now:

```php
<div @class(['underline' => $active])>
```

```php
@php
    $isActive = false;
    $hasError = true;
@endphp

<span @class([
    'p-4',
    'font-bold' => $isActive,
    'text-gray-500' => ! $isActive,
    'bg-red' => $hasError,
])></span>

<span class="p-4 text-gray-500 bg-red"></span>
```
### Single Action Controllers

If you want to create a controller with just one action, you can use `__invoke()` method and even create "invokable" controller.

Route:

```php
Route::get('user/{id}', ShowProfile::class);
```

Artisan:

```bash
php artisan make:controller ShowProfile --invokable
```

Controller:

```php
class ShowProfile extends Controller
{
    public function __invoke($id)
    {
        return view('user.profile', [
            'user' => User::findOrFail($id)
        ]);
    }
}
```
