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


### Echoing Data If It Exists

Sometimes you may wish to echo a variable, but you aren't sure if the variable has been set. We can express this in verbose PHP code like so:

```
{{ isset($name) ? $name : 'Default' }}
```

However, instead of writing a ternary statement, Blade provides you with the following convenient shortcut, which will be compiled to the ternary statement above:

```
{{ $name or 'Default' }}
```

In this example, if the $name variable exists, its value will be displayed. However, if it does not exist, the word Default will be displayed.


---

### Control Structures

Blade also provides convenient shortcuts for common PHP control structures, such as conditional statements and loops. These shortcuts provide a very clean, terse way of working with PHP control structures, while also remaining familiar to their PHP counterparts.


If Statements
You may construct if statements using the @if, @elseif, @else, and @endif directives. These directives function identically to their PHP counterparts:

```
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif
```

For convenience, Blade also provides an @unless directive:

```
@unless (Auth::check())
    You are not signed in.
@endunless
```

### Loops
In addition to conditional statements, Blade provides simple directives for working with PHP's loop structures. Again, each of these directives functions identically to their PHP counterparts:

```
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```


* When looping, you may use the loop variable to gain valuable information about the loop, such as whether you are in the first or last iteration through the loop.

> When using loops you may also end the loop or skip the current iteration:

```
@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{{ $user->name }}</li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach
```

* You may also include the condition with the directive declaration in one line:

```
@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{{ $user->name }}</li>

    @break($user->number == 5)
@endforeach

```

> The Loop Variable
When looping, a $loop variable will be available inside of your loop. This variable provides access to some useful bits of information such as the current loop index and whether this is the first or last iteration through the loop:

```
@foreach ($users as $user)
    @if ($loop->first)
        This is the first iteration.
    @endif

    @if ($loop->last)
        This is the last iteration.
    @endif

    <p>This is user {{ $user->id }}</p>
@endforeach
```

* If you are in a nested loop, you may access the parent loop's $loop variable via the parent property:

```
@foreach ($users as $user)
    @foreach ($user->posts as $post)
        @if ($loop->parent->first)
            This is first iteration of the parent loop.
        @endif
    @endforeach
@endforeach
```

The $loop variable also contains a variety of other useful properties:
```
Property	Description
$loop->index	The index of the current loop iteration (starts at 0).
$loop->iteration	The current loop iteration (starts at 1).
$loop->remaining	The iteration remaining in the loop.
$loop->count	The total number of items in the array being iterated.
$loop->first	Whether this is the first iteration through the loop.
$loop->last	Whether this is the last iteration through the loop.
$loop->depth	The nesting level of the current loop.
$loop->parent	When in a nested loop, the parent's loop variable.
```

### Comments

Blade also allows you to define comments in your views. However, unlike HTML comments, Blade comments are not included in the HTML returned by your application:

{{-- This comment will not be present in the rendered HTML --}}

## PHP

In some situations, it's useful to embed PHP code into your views. You can use the Blade @php directive to execute a block of plain PHP within your template:

```
@php
    //
@endphp
```

* While Blade provides this feature, using it frequently may be a signal that you have too much logic embedded within your template.

### Including Sub-Views
Blade's @include directive allows you to include a Blade view from within another view. All variables that are available to the parent view will be made available to the included view:

```
<div>
    @include('shared.errors')

    <form>
        <!-- Form Contents -->
    </form>
</div>
```
