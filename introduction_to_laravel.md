---
title: "Introduction to Laravel"
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
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---


## What is Laravel?

Laravel is a free, open-source, PHP web application framework with expressive, elegant syntax. Laravel makes it easy to build modern, robust, web applications with the help of its built-in features and functionality. 



## Laravel Features

Here are some key features of Laravel:
- Routing
- Blade Templating Engine
- Eloquent ORM (Object-Relational Mapping)
- Middleware
- Authentication
- Artisan CLI (Command Line Interface)

---

## Example Routing

```
Route::get('/', function () {
    return view('welcome');
});

Route::get('/about', function () {
    return view('about');
});
```
More on [routing documentation](https://laravel.com/docs/8.x/routing).

---

## Blade Templating 

```
<!DOCTYPE html>
<html>
    <head>
        <title>@yield('title')</title>
    </head>
    <body>
        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```
More on [Blade documentation](https://laravel.com/docs/8.x/blade).

--- 

## Eloquent ORM

```
// Retrieve all records
$users = User::all();

// Query with filter
$users = User::where('active', true)->get();

// Create a new record
$user = new User;
$user->name = 'John Doe';
$user->email = 'johndoe@example.com';
$user->save();
```
More on [Eloquent documentation](https://laravel.com/docs/8.x/eloquent).

---

## Middleware Example

```
public function handle($request, Closure $next)
{
    if (! $request->hasValidSignature()) {
        abort(401);
    }

    return $next($request);
}
```
More on [Middleware documentation](https://laravel.com/docs/8.x/middleware).

---

## Authentication Example

```
public function authenticate(Request $request)
{
    $credentials = $request->only('email', 'password');

    if (Auth::attempt($credentials)) {
        $request->session()->regenerate();

        return redirect()->intended('dashboard');
    }

    return back()->withErrors([
        'email' => 'The provided credentials do not match our records.',
    ]);
}
```
More on [Authentication documentation](https://laravel.com/docs/8.x/authentication).

---

## Artisan CLI Commands

```
php artisan make:model User

php artisan make:controller UserController

php artisan make:migration create_users_table
```
More on [Artisan documentation](https://laravel.com/docs/8.x/artisan).

---

## Why Choose Laravel?

- Robustness and Modularity
- Many built-in features
- Rapid development
- Easy database migration and seeding
- Large community and many packages
- MVC architecture

---

## Laravel Community

- The Laravel community is very active and helpful
- You can get help on the Laravel official Slack or the Laravel Forum
- Many resources available including podcasts, blogs, and video tutorials
- You can also keep yourself updated by following Laravel updates on social media

---

## Conclusion

- Laravel is a modern PHP web application framework designed for web developers
- Laravel's numerous built-in features make it simple to build robust web applications
- It enables high-quality, efficient, and secure web development
- Laravel provides a large and supportive community



## References 

- [Laravel Official Website](https://laravel.com/)
- [Laravel Documentation](https://laravel.com/docs/8.x)
- [Laravel GitHub Repository](https://github.com/laravel/laravel)
- [Laravel Forum](https://laracasts.com/discuss)
- [Laravel News](https://laravel-news.com/)