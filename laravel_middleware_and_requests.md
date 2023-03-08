---
title: "Laravel Middleware and Requests"
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




## Laravel Middleware and Requests

- Middleware provides a way to filter HTTP requests entering your application.
- Requests encapsulate HTTP methods (GET, POST, DELETE, PATCH, PUT, etc.) and give you tools to work with them.


---

## Middleware

- Middleware functions are invoked before a request is sent to the server. 
- They can be used for tasks like user authentication, input validation, or logging. 

```php
<?php

namespace App\Http\Middleware;

use Closure;

class ExampleMiddleware
{
    public function handle($request, Closure $next)
    {
        // Perform action before request sends
        $response = $next($request); // Call the next middleware
        // Perform action after response returns
        return $response;
    }
}
```

[Learn more here](https://laravel.com/docs/8.x/middleware)


---

## Defining Middleware

- Middleware can be defined globally, on a route, or on a controller.

```php
<?php

// Global middleware
protected $middleware = [
    \App\Http\Middleware\EncryptCookies::class,
    \Illuminate\Session\Middleware\StartSession::class,
    \Illuminate\View\Middleware\ShareErrorsFromSession::class,
    \App\Http\Middleware\VerifyCsrfToken::class,
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
];

// Route middleware
Route::get('/', function () {
    //
})->middleware('example');


```

[Learn more here](https://laravel.com/docs/8.x/middleware#registering-middleware)


---

## Middleware Groups

- Middleware groups allow you to group several middleware under a single key to simplify routes and avoid repetition. 

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Foundation\Http\Middleware\TransformsRequest;

class ExampleMiddlewareGroup
{
    protected $middleware = [
        TransformsRequest::class,
    ];
}
```

[Learn more here](https://laravel.com/docs/8.x/middleware#middleware-groups)


---

## Requests

- `Illuminate\Http\Request` represents an HTTP request.
- The request carries HTTP headers, cookies, and body content.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ExampleController extends Controller
{
    public function exampleAction(Request $request)
    {
        //
    }
}
```

[Learn more here](https://laravel.com/docs/8.x/requests)


---

## Request Methods

- You can access the HTTP verb of a request:

```php
$request->method();
```

- Or check if the request is of a certain method:

```php
$request->isMethod('post');
```

[Learn more here](https://laravel.com/docs/8.x/requests#retrieving-the-request-method)


---

## Request Input

- To retrieve a single value from the request input, you can use the get method:

```php
$value = $request->get('key');
```

- Or access input values by name:

```php
$name = $request->input('user.name');
```

[Learn more here](https://laravel.com/docs/8.x/requests#retrieving-input)


---

## Request Files

- To retrieve files from the request:

```php
$file = $request->file('file_ud');
```

- To validate uploaded files:

```php
$is_valid = $request->file('avatar')->isValid();
```

[Learn more here](https://laravel.com/docs/8.x/requests#retrieving-uploaded-files)


---

## Request Headers

- To retrieve a specific header from the request:

```php
$value = $request->header('Content-Type');
```

- Or all headers:

```php
$headers = $request->headers->all();
```

[Learn more here](https://laravel.com/docs/8.x/requests#request-information)


---

## Request Cookies

- To retrieve a specific cookie from the request:

```php
$value = $request->cookie('ck_name');
```

- Or all cookies:

```php
$cookies = $request->cookies->all();
```

[Learn more here](https://laravel.com/docs/8.x/requests#cookies)


---

## Request Query Parameters

- To retrieve a specific query parameter from the request:

```php
$value = $request->query('key');
```

- Or all query parameters:

```php
$query_params = $request->query();
```

[Learn more here](https://laravel.com/docs/8.x/requests#retrieving-query-parameters)


---

## Request Validation

- Laravel provides a convenient way to validate incoming HTTP request data.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ExampleController extends Controller
{
    public function exampleAction(Request $request)
    {
        $validated = $request->validate([
            'title' => 'required|string|max:255',
            'body' => 'required|string',
        ]);
    }
}
```

[Learn more here](https://laravel.com/docs/8.x/validation)


---

## Conclusion

- Middleware provides a way to filter HTTP requests.
- Requests encapsulate HTTP methods and provide tools to work with them.
- Use Laravel's built-in features to filter & validate incoming requests.


---

## References

- [Laravel Middleware Documentation](https://laravel.com/docs/8.x/middleware)
- [Laravel Requests Documentation](https://laravel.com/docs/8.x/requests)
- [Laravel Validation Documentation](https://laravel.com/docs/8.x/validation)


---
