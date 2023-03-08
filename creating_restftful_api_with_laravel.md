---
title: "Creating RESTful API with Laravel"
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

## What is REST API?

* REST stands for Representational State Transfer
* A RESTful API is an interface that is useful for communicating between different systems, including web and mobile applications
* APIs use HTTP requests to POST (create), PUT (update), GET (read), and DELETE data

---

## Laravel

* Laravel is a PHP framework that makes it easy to build web applications and RESTful APIs
* It provides a beautiful syntax and features like routing, ORM, migrations, and many others

---

## Setting up Laravel

* Step 1: Install Laravel using composer

```bash
composer create-project --prefer-dist laravel/laravel myapi
```

* Step 2: Run the project on local server

```bash
php artisan serve
```

* Step 3: Create a new controller

```bash
php artisan make:controller ApiController
```

---

## Creating API Endpoints

* Endpoints are routes that enable you to interact with database resources through HTTP methods
* Example:

```php
Route::get('/users', 'ApiController@index');
Route::get('/users/{id}', 'ApiController@show');
Route::post('/users', 'ApiController@store');
Route::put('/users/{id}', 'ApiController@update');
Route::delete('/users/{id}', 'ApiController@destroy');
```

---

## Handling Requests and Responses

* `$request` parameter in controllers is used to get request data
* Laravel `response()` function is used to return data in JSON format
* Example:

```php
public function index()
{
    $users = User::all();
    return response()->json($users);
}
```

---

## Querying Database

* Laravel Eloquent ORM makes it easy to interact with database records
* Example:

```php
public function show($id)
{
    $user = User::find($id);
    return response()->json($user);
}
```

---

## Input Validation

* Laravel request validation is a middleware that validates user input before processing it
* Example:

```php
public function store(Request $request)
{
    $request->validate([
        'name' => 'required',
        'email' => 'required|email|unique:users',
        'password' => 'required',
    ]);

    $user = User::create($request->all());
    return response()->json($user);
}
```

---

## Authentication

* Laravel Passport package allows for easy integration of OAuth2 API authentication
* Example:

```php
public function login(Request $request)
{
    $credentials = request(['email', 'password']);

    if (!Auth::attempt($credentials)) {
        return response()->json([
            'message' => 'Unauthorized'
        ], 401);
    }

    $user = $request->user();
    $tokenResult = $user->createToken('Personal Access Token');
    $token = $tokenResult->token;

    $token->save();

    return response()->json([
        'access_token' => $tokenResult->accessToken,
        'token_type' => 'Bearer'
    ]);
}
```

---

## Rate Limiting

* Rate limiting protects an API by restricting user access to a certain number of requests per minute
* Laravel built-in rate limiter middleware can be used to achieve this functionality

```php
Route::middleware('throttle:60,1')->group(function () {
    Route::get('/users', 'ApiController@index');
});
```

---

## Searching Resources

* Laravel query scopes can be used to implement search queries

```php
public function scopeSearch($query, $search)
{
    return $query->where('name', 'like', "%$search%")
        ->orWhere('email', 'like', "%$search%");
}
public function index(Request $request)
{
    $search = $request->query('search');
    $users = User::search($search)->get();
    return response()->json($users);
}
```

---

## API Resource Methods

* Laravel API Resource feature allows you to transform a model into a JSON representation
* Example:

```php
use Illuminate\Http\Resources\Json\JsonResource;

class UserResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
        ];
    }
}

public function index()
{
    $users = User::all();
    return UserResource::collection($users);
}
```

---

## Customizing API Resource

* Customization of API Resource enables you to add additional properties to the JSON representation of a resource
* Example:

```php
class UserResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
            'created_at' => $this->created_at,
        ];
    }
}
```

---

## API Pagination

* Laravel built-in `paginate()` method is used to paginate database results
* Example:

```php
public function index()
{
    $users = User::paginate(10);
    return UserResource::collection($users);
}
```

---

## Handling API Exceptions

* Laravel exception handler enables us to properly format and display response to API exceptions
* Example:

```php
public function show($id)
{
    $user = User::find($id);

    if (! $user) {
        throw new ModelNotFoundException('The user you are looking for doesn't exist.');
    }

    return new UserResource($user);
}
```

---

## API Versioning

* Versioning API helps keep backward compatibility and track changes
* Example:

```php
Route::middleware(['api', 'v1'])->group(function () {
    Route::get('/users', 'Api\v1\ApiController@index');
});

Route::middleware(['api', 'v2'])->group(function () {
    Route::get('/users', 'Api\v2\ApiController@index');
});
```

---

## Conclusion

* RESTful API with Laravel makes it easy to build, test, and deploy APIs
* The Laravel framework allows you to focus on the business logic of your application and not re-inventing the wheel
* Laravel also has a great documentation to help you through every step

---

## References

1. [Laravel Official Documentation](https://laravel.com/docs)
2. [REST API with Laravel: A Beginner's Guide](https://scotch.io/@danielcwilson/rest-api-with-laravel-a-beginners-guide)
3. [API Resource with Laravel](https://laravel.com/docs/8.x/eloquent-resources)
4. [Laravel Sanctum: API Authentication Made Simple](https://laravel.com/docs/8.x/sanctum)
5. [Laravel API Versioning](https://laravel.com/docs/8.x/routing#route-group-versioning)
