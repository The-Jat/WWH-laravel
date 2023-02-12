## Layout in Blade Template.

## Templating Inheritance

Defining a Layout

The two primary benefits of Blade are as follows:

1. Inheritance
2. Sections

Most web applications maintain the same layouts across various pages. It is more convenient to define the layout as a Blade view.

web.php
```
Route::get('/', function () {
    return view('home');
});

Route::get('/about', function () {
    return view('about');
});
```

/layouts/main.blade.php
```
@include('layouts.header')
<div class="container">
    @yield('main-section')
</div>
@include('layouts.footer')
```

layouts/header.blade.php

```
<html>
    <h1>
        This is Header
    </h1>
</html>
```

layouts/footer.blade.php

```
<h2>
    This is Footer
</h2>
```

home.blade.php
```
@extends('layouts.main')

@section("main-section")
<h1>
    Home Page
</h1>
@endsection
```

about.blade.php
```
@extends('layouts.main')

@section("main-section")
<h1>
    About
</h1>
@endsection
```


---
