## What is Laravel Component?

A component is a piece of code, which we can reuse in any blade template. It’s something similar to sections, layouts, and includes. For example, we use the same header for each template, so we can create a Header component, which we can reuse.

Another use of components for better understanding is like you need to use a Sign-Up button on the website at many places, like in the header, footer, or any other place on the website. Then make a component of that button code and reuse it.

## How to Make Component in Laravel.

For example, we create a Header component. Below artisan command is used to create the header component.
```
php artisan make:component component_name
```

This command makes two files in your laravel project.

One PHP file is created with the name of component_name.php inside app/http/View/Components directory.

And another HTML file is created with the name of component.blade.php inside resources/views/components/ directory.

You can also make components with in subdirectories like:
```
php artisan make:component Forms/Button
```
This command will create a button component in App\View\Components\Forms directory and the blade file will be placed in the resources/views/components/forms directory.

This syntax is used to rendered component in HTML file:
```
<x-ComponentName/>
```
