# Middleware in Laravel

Middleware in Laravel provides a way to filter and modify HTTP requests and responses. It acts as a bridge between the server and your application, allowing you to perform tasks such as authentication, authorization, request transformation, and more. In this README, we will cover the basics of using middleware in Laravel with examples.

## Table of Contents
- [Introduction to Middleware](#introduction-to-middleware)
- [Creating Middleware](#creating-middleware)
- [Registering Middleware](#registering-middleware)
- [Applying Middleware](#applying-middleware)
- [Middleware Parameters](#middleware-parameters)
- [Global Middleware](#global-middleware)


## Introduction to Middleware

Middleware functions as layers of logic that intercept requests and responses before they reach your application's routes. It allows you to perform operations on the request, modify the response, or terminate the request and return a response immediately.

Middleware can be useful for tasks such as:

- Authentication: Verifying if a user is authenticated before accessing protected routes.
- Authorization: Checking if a user has the necessary permissions to perform a certain action.
- Request Validation: Validating incoming request data to ensure it meets specific criteria.
- Logging: Recording request details or performing logging operations.
- Response Manipulation: Modifying the response before it is sent back to the client.
- CORS Handling: Adding headers to handle Cross-Origin Resource Sharing.

## Creating Middleware

To create a new middleware class, you can use the `make:middleware` Artisan command:

```shell
php artisan make:middleware MyMiddleware
```

This will generate a new middleware class in the `app/Http/Middleware` directory. Open the newly created middleware class and locate the `handle` method. This method contains the logic that will be executed for each request.

```php
namespace App\Http\Middleware;

use Closure;

class MyMiddleware
{
    public function handle($request, Closure $next)
    {
        // Perform middleware logic

        return $next($request);
    }
}
```

In the `handle` method, you can perform any necessary operations on the incoming request. The `$next` parameter represents the next middleware or the final request handler. You can choose to modify the request, perform checks, or terminate the request and return a response.

### Example: Logging Middleware

Let's create a middleware that logs information about incoming requests. This can be useful for debugging or auditing purposes. Create a new middleware using the `make:middleware` command:

```shell
php artisan make:middleware LogRequestsMiddleware
```

Open the generated `LogRequestsMiddleware` class and update the `handle` method:

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\Log;

class LogRequestsMiddleware
{
    public function handle($request, Closure $next)
    {
        Log::info('Incoming Request', [
            'url' => $request->fullUrl(),
            'method' => $request->method(),
            'ip' => $request->ip(),
        ]);

        return $next($request);
    }
}
```

In the example above, we use the `Log` facade to log information about the incoming request. We log the URL, request method, and IP address. You can customize the logging behavior according to your needs.

## Registering Middleware

After creating a middleware class, you need to register it in the Laravel application. Open the `app/Http/Kernel.php` file and locate the `$routeMiddleware` property. This property contains the middleware classes that can be applied to routes.

```php
protected $routeMiddleware = [
    'my-middleware' => \App\Http\Middleware\MyMiddleware::class,
];
```

In the example above

, we register the `MyMiddleware` class with the key `'my-middleware'`. This key will be used to apply the middleware to specific routes or groups.

## Applying Middleware

You can apply middleware to routes or route groups in your application. Middleware can be applied in several ways:

### 1. Applying Middleware to a Route

To apply middleware to a specific route, you can use the `middleware` method in your route definition:

```php
Route::get('/example', function () {
    // Route logic
})->middleware('my-middleware');
```

In the example above, the `'my-middleware'` middleware will be executed before processing the `/example` route.

### 2. Applying Middleware to a Route Group

To apply middleware to a group of routes, you can use the `middleware` method within a route group:

```php
Route::middleware('my-middleware')->group(function () {
    Route::get('/route1', function () {
        // Route 1 logic
    });

    Route::get('/route2', function () {
        // Route 2 logic
    });
});
```

In the example above, the `'my-middleware'` middleware will be applied to both `/route1` and `/route2`.

### 3. Applying Middleware to Controller Actions

If you prefer to apply middleware to controller actions, you can use the `middleware` method within the controller's constructor:

```php
public function __construct()
{
    $this->middleware('my-middleware');
}
```

In this case, the `'my-middleware'` middleware will be applied to all actions within the controller.

### Example: Authentication Middleware

Let's create a middleware for authentication that ensures users are logged in before accessing certain routes. Create a new middleware using the `make:middleware` command:

```shell
php artisan make:middleware AuthenticateMiddleware
```

Open the generated `AuthenticateMiddleware` class and update the `handle` method:

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\Auth;

class AuthenticateMiddleware
{
    public function handle($request, Closure $next)
    {
        if (!Auth::check()) {
            return redirect('/login');
        }

        return $next($request);
    }
}
```

In the example above, we use the `Auth` facade to check if the user is authenticated. If not, we redirect the user to the login page. Modify the logic according to your authentication requirements.

## Middleware Parameters

Middleware can also accept additional parameters if needed. To pass parameters to middleware, you can specify them when applying the middleware:

```php
Route::get('/example', function () {
    // Route logic
})->middleware('my-middleware:param1,param2');
```

In the example above, the `'my-middleware'` middleware will receive the parameters `'param1'` and `'param2'`.

To handle these parameters in the middleware, you can update the `handle` method signature:

```php
public function handle($request, Closure $next, $param1, $param2)
{
    // Middleware logic

    return $next($request);
}
```

Make sure to include the parameters in the `handle` method signature in the same order as they were passed.

### Example: Role-based Authorization Middleware

Let's create a middleware that checks if the authenticated user has a specific role to access certain routes. Create a new middleware using the `make:middleware` command:

```shell
php artisan make:middleware RoleMiddleware
```

Open the generated `RoleMiddleware` class and update the `handle` method:

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\Auth;

class RoleMiddleware
{
    public function handle($request

, Closure $next, $role)
    {
        if (!Auth::check() || !Auth::user()->hasRole($role)) {
            return abort(403);
        }

        return $next($request);
    }
}
```

In the example above, we assume that the `User` model has a `hasRole` method to check if the user has a specific role. If the user is not authenticated or doesn't have the required role, we return a 403 Forbidden response.

## Global Middleware

In addition to applying middleware to specific routes or groups, you can also specify global middleware that will be applied to all incoming requests. These global middleware are executed for every request in your application.

To define global middleware, open the `app/Http/Kernel.php` file and locate the `$middleware` property. This property contains the list of global middleware classes.

```php
protected $middleware = [
    \App\Http\Middleware\EncryptCookies::class,
    \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
    // ...
];
```

In the example above, the `EncryptCookies` and `AddQueuedCookiesToResponse` middleware are applied to every request.
