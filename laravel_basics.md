---
title: "Laravel Basics"
author: "Vladimir Lelicanin - SAE Institute"
format:
  revealjs:
    theme: default
    title-slide-attributes:
        data-background-image: https://keystoneacademic-res.cloudinary.com/image/upload/f_auto/q_auto/g_auto/w_256/element/15/156456_sae.jpg
        data-background-position: top left
        data-background-repeat: no-repeat
        data-background-size: 100px
    transition: fade
    footer: "Vladimir Lelicanin - SAE Institute Belgrade"
    margin: 0.2
    auto-animate: true
    preview-links: auto
    link-external-newwindow: true
    scrollable: true
    embed-resources: false
    chalkboard: true
    multiplex: true
    slide-number: true
    slide-level: 1
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---


## MVC Architecture

- **Model-View-Controller** (MVC) is a software design pattern for implementing user interfaces
- Separates application into three interconnected parts: 
1. **Model** - represents the application logic
2. **View** - represents the UI
3. **Controller** - receives user input and directs model and view to perform actions.

---

![MVC pattern](https://upload.wikimedia.org/wikipedia/commons/a/a0/MVC-Process.svg)

---

- In Laravel, the **Model** corresponds to database tables 
- The **View** is generally presented as **HTML**, CSS, and JS
- The **Controller** accepts input and sends commands to the Model or View accordingly

---

## New Laravel Project


```bash
$ composer create-project --prefer-dist laravel/laravel blog
```

- Creates new Laravel Project in directory 'blog'
- Sets up all basic Laravel dependencies
- Configures basic settings based on the variables you provide during setup

---

## Routing

- Matches a URI (or a set of URIs) to a specific action. 
- In Laravel, routes are defined in the `routes/web.php` file
- Basic example:

```php
Route::get('/hello', function () {
    return 'Hello, World!';
});
```

- Visit `/hello` in your web browser to see the response. 

---

- The `get` method accepts two parameters:
  - URI
  - Closure (or a controller action)

- Laravel provides a `Route` facade to access routing functionalities 

---

## Controllers

- Controllers are used to group related request handling logic into a single class. 

- Create a new controller:

```bash
$ php artisan make:controller MyController
```

- Method example:

```php
public function myMethod() 
{
   // Request handling logic
}
```

---

- Controllers can return views, files or JSON data
- Example:

```php
public function returnJson() 
{
    return response()->json([
        'name' => 'John Doe',
        'status' => 'active'
    ]);
}
```

---

- To link routes to the controller method:

```php
Route::get('/hello', 'MyController@myMethod');
```

- The first parameter is the URI, the second is the controller and the method joined by '@'

---

## Blade

- Blade is a templating engine provided with Laravel, 
- Plain PHP in templates is possible, but Blade makes things simpler.
- Blade templates use `.blade.php` extension 

---

- Blade templates allow you to extend a base template

- Example of `app.blade.php`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>@yield('title')</title>
</head>
<body>
 
    <div class="container">
        @yield('content')
    </div>
 
</body>
</html>
```

- `@yield` directives define where sections will appear 

---

- Example of Child Template

```html
@extends('layouts.app')
 
@section('title', 'Page Title')
 
@section('content')
    <p>This is my body content.</p>
@endsection
``` 

- `@extends` specifies the parent view
- `@section` specifies the content of each section

---

- Blade comes with other features like looping, if statements and more 
- Example:

```html
@if (count($users) === 1)
    <p>I have one user!</p>
@elseif (count($users) > 1)
    <p>I have multiple users!</p>
@else
    <p>I have no users!</p>
@endif
```

- `@if`, `@elseif`, `@else` check a condition
- `@foreach`, `@for` iterate over an array/object 

---

## References



- [Laravel Documentation](https://laravel.com/docs/10.x)
- [MVC Architecture](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)
- [Blade Templating](https://laravel.com/docs/10.x/blade)
