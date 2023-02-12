## Controller

Controller classes can be used in defining all of your request handling logic.

Controllers can group related request handling logic into a single class.

Controllers are stored in the app/Http/Controllers directory.

### Creating a Controller

Open the command prompt or terminal based on the operating system you are using and type the following command to create the controller using the Artisan CLI (Command Line Interface).

```
php artisan make:controller <controller-name>
```

### passing the control to controller on route hitting.

Create a new demoController
> php artisan make:controller DemoController

web.php
```
use App\Http\Controllers\DemoController; // include the newly created demoController class.

Route::get('/', [DemoController::class, 'index']);

```

DemoController.php
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class DemoController extends Controller
{
    public function index(){
        return view('home'); // it will return the home view.
    }
}

```
