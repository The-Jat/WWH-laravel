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

## @stack and @push

Like we have seen that in above example

Blade allows you to push to named stacks which can be rendered somewhere else in another view or layout. This can be particularly useful for specifying any JavaScript libraries required by your child views:

```
@push('scripts')
    <script src="/example.js"></script>
@endpush
```

You may push to a stack as many times as needed. To render the complete stack contents, pass the name of the stack to the @stack directive:

```
<head>
    <!-- Head Contents -->

    @stack('scripts')
</head>
```

or another example that we want to change the title of the page in the common header based on the child view.

layouts/header.blade.php
```
<html>
    <head>
        @stack('title')
    </head>
    <h1>
        This is Header
    </h1>
</html>
```

main.blade.php
```
@include('layouts.header')
<div class="container">
    @yield('main-section')
</div>
@include('layouts.footer')
```

home.blade.php
```
@extends('layouts.main')

@push('title')
    <title> Home
    </title>
@endpush

@section("main-section")
<h1>
    Home Page
</h1>
@endsection
```

---
