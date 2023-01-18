## Routes

## What Is a Route?
The route is a way of creating a request URL for your application. These URLs do not have to map to specific files on a website, and are both human readable and SEO friendly.

In Laravel, routes are created inside the routes folder. They are created in the web.php file for websites. And for APIs, they are created inside api.php.

> routes/web.php     -> All web routes are defined here

> There is 4 types of route methods GET, POST, PUT, DELETE

### Syntax: To write route is as given below:

```
// Syntax of a route
Route::request_type('/url', 'function()');
```

### Here is how the root route for web in web.php looks like:
```

// Creating a new route, This one is root route
Route::get('/', function() {
    return 'Hey ! Hello';
});

```
> Output :-

![Routes](Routes_simple.png)


### Here is how the custom route for web in web.php looks like:

```
// Creating a new route
Route::get('/sayJaat', function() {
    return 'Hey ! Mr. Jaat';
});

```
> Output :-

![Routes non root](Routes_non_root.png)

---

## Returning Web-page:

Instead of just returning strings, we are going to return webpages when someone visits a route. Let’s see how we can do that. First of all create a file called welcome.blade.php in resources/views. In Laravel we have a built-in templating engine called Blade thus we write all of our webpages in *.blade.php not *.html.


> routes/web.php

```
// Creating a new route
Route::get('/', function() {
    return view('welcome');
});
```
> resourcs/view/welcome.blade.php

```
<!DOCTYPE html>
<html lang="en">
    <body>
        <h1>Hello! World from the View</h1>
    </body>
</html>
```
