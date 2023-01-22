## Blade Template

### Introduction

Blade is the simple, yet powerful templating engine provided with Laravel. Unlike other popular PHP templating engines, Blade does not restrict you from using plain PHP code in your views. In fact, all Blade views are compiled into plain PHP code and cached until they are modified, meaning Blade adds essentially zero overhead to your application. Blade view files use the .blade.php file extension and are typically stored in the resources/views directory.

### Displaying Data

You may display data passed to your Blade views by wrapping the variable in curly braces. For example, given the following route:

> Web.php
```
Route::get('greeting', function () {
    return view('welcome', ['name' => 'MaK']);
});
```

> welcome.blade.php

You may display the contents of the name variable like so:

```
<!DOCTYPE html>
<html lang="en">
    <p>{{$name}}</p>
</html>
``
Output = MaK



