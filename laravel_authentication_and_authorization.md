---
title: "Laravel Authentication and Authorization"
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
    slide-level: 1
    chalkboard: true
    multiplex: true
    slide-number: true
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---


# Laravel Authentication 

- Authentication is the process of verifying the identity of a user.
- Laravel comes with a built-in authentication system.
- Uses guard-based authentication, provides multiple ways to authenticate users.

Documentation: [https://laravel.com/docs/8.x/authentication](https://laravel.com/docs/8.x/authentication)



# Guard-based authentication 

- Guard defines how users are authenticated for each request.
- Laravel provides different types of guards based on user type, multiple auth systems.
- Can create own custom guards.

Example: 
```php
// Default authentication guard
auth()->guard();

// Custom guard with session and token stored in a database
auth()->guard('api');
```

Documentation: [https://laravel.com/docs/8.x/authentication#adding-custom-guards](https://laravel.com/docs/8.x/authentication#adding-custom-guards)

---

## Authentication controllers 

- Laravel provides out-of-the-box controllers for authentication.
- /login route shows a login form.
- Role-based authentication provided by @middleware and @auth.

Example: 

```php
public function __construct()
{
    $this->middleware('auth');
}
```

Documentation: [https://laravel.com/docs/8.x/authentication#authentication-controllers](https://laravel.com/docs/8.x/authentication#authentication-controllers)

---

## Authentication routes 

- Defines authentication routes in 'routes/web.php'.
- Includes routes for login, logout, registration, and password reset.
- Redirects to home (/) route after authenticating.

Example: 

```php
Route::get('/login', [LoginController::class, 'showLoginForm'])->name('login');
```

Documentation: [https://laravel.com/docs/8.x/authentication#included-authentication-routes](https://laravel.com/docs/8.x/authentication#included-authentication-routes)

---

## Laravel Authorization 

- Authorization refers to the process of determining what users can and cannot access.
- Laravel provides a simple way to authorize user access.

Documentation: [https://laravel.com/docs/8.x/authorization](https://laravel.com/docs/8.x/authorization)

---

## Gates 

- Gates define user access to specific actions.
- Can define gates for users, models, or policies.

Example: 

```php
Gate::define('update-post', function ($user, $post) {
    return $user->id === $post->user_id;
});
```

Documentation: [https://laravel.com/docs/8.x/authorization#gates](https://laravel.com/docs/8.x/authorization#gates)

---

## Policies 

- Policies define user authorization for specific models classes.
- Can define policies in each model, and override in a specific controller.

Example: 

```php
class PostPolicy
{
    use HandlesAuthorization;

    public function delete(User $user, Post $post)
    {
        return $user->id === $post->user_id;
    }
}
```

Documentation: [https://laravel.com/docs/8.x/authorization#creating-policies](https://laravel.com/docs/8.x/authorization#creating-policies)

---

## Policy authorization 

- Policies used to authorize user access to a controller action.
- Laravel automatically generates the policy methods when the policy exists.
- Can call policy method or use @can blade directive.

Example:

```php
public function delete(Post $post)
{
    $this->authorize('delete', $post);
}
```

Documentation: [https://laravel.com/docs/8.x/authorization#authorizing-actions-via-policies](https://laravel.com/docs/8.x/authorization#authorizing-actions-via-policies)

---

## Middleware-based authorization 

- Middleware-based authorization can be used to allow or deny user access to a route.
- Uses the same authorization techniques as for policies and gates.

Example: 

```php
Route::middleware('can:delete-post,post')->delete('/post/{post}', function (Post $post) {
    $post->delete();
});
```

Documentation: [https://laravel.com/docs/8.x/authorization#middleware-usage](https://laravel.com/docs/8.x/authorization#middleware-usage)

---

## Authentication and Authorization example 

- Authentication and authorization example with policies.
- Authorization using post policies.

Example: 

```php
public function delete(Post $post)
{
    $this->authorize('delete', $post);
    $post->delete();
}
```

Documentation: [https://laravel.com/docs/8.x/authorization#creating-policies](https://laravel.com/docs/8.x/authorization#creating-policies)

---

## Middleware-based Authorization example 

- Middleware-based authorization example.
- User can delete only own posts.

Example: 

```php
Route::middleware(['auth', 'can:delete-post,post'])->delete('/post/{post}', function (Post $post) {
    $post->delete();
});
```

Documentation: [https://laravel.com/docs/authorization#middleware-usage](https://laravel.com/docs/authorization#middleware-usage)

---

## Conclusion 

- Laravel has a robust authentication and authorization system.
- Authentication using guard-based system, implements different guards.
- Authorization using gates, policies, and middleware-based authorization.

--- 

## References 

- Laravel authentication documentation: [https://laravel.com/docs/authentication](https://laravel.com/docs/authentication)
- Laravel authorization documentation: [https://laravel.com/docs/authorization](https://laravel.com/docs/8.x/authorization)
- Laravel gates documentation: [https://laravel.com/docs/8.x/authorization#gates](https://laravel.com/docs/8.x/authorization#gates)
- Laravel policies documentation: [https://laravel.com/docs/8.x/authorization#creating-policies](https://laravel.com/docs/8.x/authorization#creating-policies)
