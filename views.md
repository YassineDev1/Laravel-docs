# Laravel Views

Views in Laravel are used to generate the HTML representation of your application's user interface. They separate the presentation logic from the application logic, making it easier to maintain and update your application's UI. In this README, we will explore the basics of using views in the latest version of Laravel.

## Table of Contents
- [Creating Views](#creating-views)
- [Passing Data to Views](#passing-data-to-views)
- [Including Sub-Views](#including-sub-views)
- [Blade Templating](#blade-templating)

## Creating Views

In Laravel, views are typically stored in the `resources/views` directory. You can create a new view by creating a new file with the `.blade.php` extension within this directory. For example, to create a view called `welcome.blade.php`, you would create a file at `resources/views/welcome.blade.php`.

## Passing Data to Views

To pass data to a view, you can use the `view` function provided by Laravel. The first argument of the `view` function is the name of the view you want to render, and the second argument is an array of data that you want to make available to the view.

```php
Route::get('/hello', function () {
    $name = 'Yassine';
    return view('welcome', ['name' => $name]);
});
```

In the above example, we pass the `$name` variable to the `welcome` view, making it accessible within the view template.

## Including Sub-Views

Sometimes, you may want to include the contents of one view within another view. Laravel provides the `@include` directive to achieve this. The `@include` directive includes the contents of the specified view and allows you to pass data to the included view.

```php
<!-- resources/views/layout.blade.php -->
<html>
    <body>
        <header>
            @include('partials.header')
        </header>
        
        <div class="content">
            @yield('content')
        </div>
        
        <footer>
            @include('partials.footer')
        </footer>
    </body>
</html>
```

In the example above, the `@include` directive is used to include the `partials.header` and `partials.footer` views within the `layout` view. The `@yield` directive is used to define a section that can be overridden by child views.

## Blade Templating

Laravel's Blade templating engine provides a concise and expressive syntax for working with views. Blade allows you to write clean, readable templates with features like control structures, template inheritance, and more.

```php
<!-- resources/views/welcome.blade.php -->
<html>
    <body>
        <h1>Welcome, {{ $name }}!</h1>
    </body>
</html>
```

In the above example, the `{{ $name }}` syntax is used to output the value of the `$name` variable within the HTML.

