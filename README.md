# Laravel-docs


## Views
### There are multiple ways to return a view with variables

```php
// First way ->with()
return view('index')
    ->with('projects', $projects)
    ->with('tasks', $tasks)

// Second way - as an array
return view('index', [
        'projects' => $projects,
        'tasks' => $tasks
    ]);

// Third way - the same as second, but with variable
$data = [
    'projects' => $projects,
    'tasks' => $tasks
];
return view('index', $data);

// Fourth way - the shortest - compact()
return view('index', compact('projects', 'tasks'));
```

## Blade templates
### Blade directive to add true/false conditions

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
## Controllers
### Single Action Controllers

If you want to create a controller with just one action, you can use `__invoke()` method and even create "invokable" controller.

Route:

```php
Route::get('user/{id}', ShowProfile::class);
```

Artisan:

```command line
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

### Controller groups

Instead of using the controller in each route, consider using a route controller group. Added to Laravel since v8.80

```php
// Before
Route::get('users', [UserController::class, 'index']);
Route::post('users', [UserController::class, 'store']);
Route::get('users/{user}', [UserController::class, 'show']);
Route::get('users/{user}/ban', [UserController::class, 'ban']);
// After
Route::controller(UsersController::class)->group(function () {
    Route::get('users', 'index');
    Route::post('users', 'store');
    Route::get('users/{user}', 'show');
    Route::get('users/{user}/ban', 'ban');
});
```
## Routing

### to_route() helper function

Laravel 9 provides shorter version of `redirect()->route()`, take a look on the following code:

```php
// Old way
Route::get('redirectRoute', function() {
    return redirect()->route('home');
});

// Post Laravel 9
Route::get('redirectRoute', function() {
    return to_route('home');
});

```
### Route group within a group

In Routes, you can create a group within a group, assigning a certain middleware only to some URLs in the "parent" group.

```php
Route::group(['prefix' => 'account', 'as' => 'account.'], function() {
    Route::get('login', [AccountController::class, 'login']);
    Route::get('register', [AccountController::class, 'register']);
    Route::group(['middleware' => 'auth'], function() {
        Route::get('edit', [AccountController::class, 'edit']);
    });
});
```
### Route view

You can use `Route::view($uri , $bladePage)` to return a view directly, without having to use controller function.

```php
//this will return home.blade.php view
Route::view('/home', 'home');
```

### Query string parameters to Routes

If you pass additional parameters to the route, in the array, those key / value pairs will automatically be added to the generated URL's query string.

```php
Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');

$url = route('profile', ['id' => 1, 'photos' => 'yes']); // Result: /user/1/profile?photos=yes
```
### Route resources grouping

If your routes have a lot of resource controllers, you can group them and call one Route::resources() instead of many single Route::resource() statements.

```php
Route::resources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);
```
## Middleware
### Pass arguments to middleware

You can pass arguments to your middleware for specific routes by appending ':' followed by the value. For example, I'm enforcing different authentication methods based on the route using a single middleware.

```php
Route::get('...')->middleware('auth.license');
Route::get('...')->middleware('auth.license:bearer');
Route::get('...')->middleware('auth.license:basic');
```

```php
class VerifyLicense
{
    public function  handle(Request $request, Closure $next, $type = null)
    {
        $licenseKey  = match ($type) {
            'basic'  => $request->getPassword(),
            'bearer' => $request->bearerToken(),
            default  => $request->get('key')
        };

        // Verify license and return response based on the authentication type
    }
}
```
## Session
### Get value from session and forget

If you need to grab something from the Laravel session, then forget it immediately, consider using `session()->pull($value)`. It completes both steps for you.

```php
// Before
$path = session()->get('before-github-redirect', '/components');

session()->forget('before-github-redirect');

return redirect($path);

// After
return redirect(session()->pull('before-github-redirect', '/components'))
```

### Sessions has() vs exists() vs missing()

Do you know about `has`, `exists` and `missing` methods in Laravel session?

```php
// The has method returns true if the item is present & not null.
$request->session()->has('key');

// THe exists method returns true if the item ir present, event if its value is null
$request->session()->exists('key');

// THe missing method returns true if the item is not present or if the item is null
$request->session()->missing('key');
```
