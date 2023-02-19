## Middleware

> It provides the method to filter HTTP requests that get entered into your project. 

Middleware acts as a layer between the user and the request. It means that when the user requests the server then the request
will pass through the middleware, and then the middleware verifies whether the request is authenticated or not.
If the user's request is authenticated then the request is sent to the backend. If the user request is not authenticated,
then the middleware will redirect the user to the login screen.

Through middleware, you can inspect and take action before and/or after a specific request is handled and processed by Laravel. For instance, you can have middleware that runs for a specific route, the members' area route, to check whether the user trying to access this route is indeed a member. Accordingly, the middleware can take action to either redirect the user to the membership page to become a member or allow the request to go through for a registered member.


### Creating Middleware

You can create your middleware by running the syntax below:
```
php artisan make:middleware <middleware_name>

// for example 

php artisan make:middleware CheckAge
```

+ You can see this path location app/Http/Middleware
+ for our example the genrated path file is = app/Http/Middleware/CheckAge.php


app/Http/Middleware/CheckAge.php
```
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure(\Illuminate\Http\Request): (\Illuminate\Http\Response|\Illuminate\Http\RedirectResponse)  $next
     * @return \Illuminate\Http\Response|\Illuminate\Http\RedirectResponse
     */
    public function handle(Request $request, Closure $next)
    {
        return $next($request);
    }
}

```

> Change the above code as below to filter by age.

```
<?php

namespace App\Http\Middleware;

use Closure;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($request->age <= 200) {
            return redirect('home');
        }

        return $next($request);
    }

}

/* 
As you can see, if the given age is less than or equal to 200,
the middleware will return an HTTP redirect to the client; otherwise, 
the request will be passed further into the application.

To pass the request deeper into the application (allowing the middleware to "pass"), 
simply call the $next callback with the $request.
*/
```

#### Before & After Middleware

Whether a middleware runs before or after a request depends on the middleware itself. For example, the following middleware would perform some task before the request is handled by the application:

```
<?php

namespace App\Http\Middleware;

use Closure;

class BeforeMiddleware
{
    public function handle($request, Closure $next)
    {
        // Perform action

        return $next($request);
    }
}
```

However, this middleware would perform its task after the request is handled by the application:

```
<?php

namespace App\Http\Middleware;

use Closure;

class AfterMiddleware
{
    public function handle($request, Closure $next)
    {
        $response = $next($request);

        // Perform action

        return $response;
    }
}

```

### Registering Middleware

Before using any middleware, you have to register it.

1. global Middleware.

Global middlewares are those that will be running during every HTTP request of your application.
In the $middleware property of your app/Http/Kernel.php class, you can list all the global middleware for your project.

```
/**
     * The application's global HTTP middleware stack.
     *
     * These middleware are run during every request to your application.
     *
     * @var array<int, class-string|string>
     */
    protected $middleware = [
        // \App\Http\Middleware\TrustHosts::class,
        \App\Http\Middleware\TrustProxies::class,
        \Fruitcake\Cors\HandleCors::class,
        \App\Http\Middleware\PreventRequestsDuringMaintenance::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
    ];
```

2. Assigning Middleware To Routes

If you would like to assign middleware to specific routes, you should first assign the middleware a key in your app/Http/Kernel.php file. By default, the $routeMiddleware property of this class contains entries for the middleware included with Laravel. To add your own, simply append it to this list and assign it a key of your choosing. For example:

```
// Within App\Http\Kernel Class...

protected $routeMiddleware = [
    'auth' => \Illuminate\Auth\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
];
```

#### Once the middleware has been defined in the HTTP kernel, you may use the middleware method to assign middleware to a route:

```
Route::get('admin/profile', function () {
    //
})->middleware('auth');
You may also assign multiple middleware to the route:

Route::get('/', function () {
    //
})->middleware('first', 'second');
When assigning middleware, you may also pass the fully qualified class name:

use App\Http\Middleware\CheckAge;

Route::get('admin/profile', function () {
    //
})->middleware(CheckAge::class);

```

#### Middleware Groups

Sometimes you may want to group several middleware under a single key to make them easier to assign to routes. You may do this using the $middlewareGroups property of your HTTP kernel.

Out of the box, Laravel comes with web and api middleware groups that contains common middleware you may want to apply to your web UI and API routes:

```
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],

    'api' => [
        'throttle:60,1',
        'auth:api',
    ],
];

```

+ Middleware groups may be assigned to routes and controller actions using the same syntax as individual middleware. Again, middleware groups simply make it more convenient to assign many middleware to a route at once:

```
Route::get('/', function () {
    //
})->middleware('web');

Route::group(['middleware' => ['web']], function () {
    //
});

```

{tip} Out of the box, the web middleware group is automatically applied to your routes/web.php file by the RouteServiceProvider.

#### Middleware Parameters

Middleware can also receive additional parameters. For example, if your application needs to verify that the authenticated user has a given "role" before performing a given action, you could create a CheckRole middleware that receives a role name as an additional argument.

Additional middleware parameters will be passed to the middleware after the $next argument:

```
<?php

namespace App\Http\Middleware;

use Closure;

class CheckRole
{
    /**
     * Handle the incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @param  string  $role
     * @return mixed
     */
    public function handle($request, Closure $next, $role)
    {
        if (! $request->user()->hasRole($role)) {
            // Redirect...
        }

        return $next($request);
    }

}
```

Middleware parameters may be specified when defining the route by separating the middleware name and parameters with a :. Multiple parameters should be delimited by commas:

```
Route::put('post/{id}', function ($id) {
    //
})->middleware('role:editor');

```

#### Terminable Middleware

Sometimes a middleware may need to do some work after the HTTP response has been sent to the browser. For example, the "session" middleware included with Laravel writes the session data to storage after the response has been sent to the browser. If you define a terminate method on your middleware, it will automatically be called after the response is sent to the browser.

```
<?php

namespace Illuminate\Session\Middleware;

use Closure;

class StartSession
{
    public function handle($request, Closure $next)
    {
        return $next($request);
    }

    public function terminate($request, $response)
    {
        // Store the session data...
    }
}
```

The terminate method should receive both the request and the response. Once you have defined a terminable middleware, you should add it to the list of global middleware in your HTTP kernel.

When calling the terminate method on your middleware, Laravel will resolve a fresh instance of the middleware from the service container. If you would like to use the same middleware instance when the handle and terminate methods are called, register the middleware with the container using the container's singleton method.
