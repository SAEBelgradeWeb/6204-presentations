---
title: "Laravel Advanced Eloquent"
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


# Laravel Advanced Eloquent



## Introduction

- Eloquent ORM is a PHP library used for working with databases
- Allows high-level queries instead of writing SQL statements
- Provides relation mappings, eager loading, and other useful features
- Advanced Eloquent takes advantage of these features for more complex situations

---

## Model Scopes

- A method on a model that returns a query builder instance
- Can be chained onto a query to apply logic globally
- Great for repetitive queries or to abstract complex queries
- Example:

```php
class Post extends Model
{
  public function scopePublished($query)
  {
    return $query->where('published', true);
  }
}

// Usage:
$posts = Post::published()->get();
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent#query-scopes)

---

## Global Scopes

- Similar to model scopes but applied to every query automatically
- Can be useful for multi-tenant applications or certain security requirements
- Example:

```php
class TenantScope implements Scope
{
  public function apply(Builder $builder, Model $model)
  {
    $builder->where('tenant_id', auth()->user()->tenant_id);
  }
}

// In the model:
protected static function boot()
{
  parent::boot();
  static::addGlobalScope(new TenantScope());
}

// Usage:
$posts = Post::all();
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent#global-scopes)

---

## Polymorphic Relations

- One table can belong to multiple other tables with a single relationship
- Great for keeping records flexible
- Example:

```php
class Image extends Model
{
  public function imageable()
  {
    return $this->morphTo();
  }
}

class Post extends Model
{
  public function images()
  {
    return $this->morphMany(Image::class, 'imageable');
  }
}

class Product extends Model
{
  public function images()
  {
    return $this->morphMany(Image::class, 'imageable');
  }
}

// Usage:
$postImages = $post->images;
$productImages = $product->images;
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent-relationships#polymorphic-relationships)

---

## Accessors and Mutators

- Used to modify or retrieve Eloquent model attributes
- Great for formatting data before or after it's saved to the database
- Example:

```php
class User extends Model
{
  public function getNameAttribute($value)
  {
    return ucfirst($value);
  }
  
  public function setPasswordAttribute($value)
  {
    $this->attributes['password'] = bcrypt($value);
  }
}

// Usage:
$user = new User();
$user->name = 'john';
$user->password = 'password123';
$user->save();
echo $user->name; // John
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent-mutators#defining-an-accessor)

---

## Model Observers

- Listens for specific Eloquent model events
- Can be used to perform additional actions during those events
- Example:

```php
class UserObserver
{
  public function created(User $user)
  {
      Mail::to($user->email)->send(new WelcomeEmail());
  }
}

// In a service provider:
public function boot()
{
  User::observe(UserObserver::class);
}

// Usage:
$user = new User();
$user->name = 'Jane';
$user->email = 'jane@gmail.com';
$user->password = 'password123';
$user->save(); // Sends welcome email to jane@gmail.com
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent#observers)

---

## Eager Loading

- Used to load relationships with fewer queries
- Can help performance by reducing database trips
- Example:

```php
// Without eager loading:
$users = User::all();
foreach ($users as $user) {
  echo $user->posts->count(); // N+1 problem
}

// With eager loading:
$users = User::with('posts')->get();
foreach ($users as $user) {
  echo $user->posts->count(); // No additional queries
}
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent-relationships#eager-loading)

---

## Query Optimization

- Use `select`, `where`, and `orderBy` to optimize queries
- Use the `DB::enableQueryLog()` and `DB::getQueryLog()` methods to analyze queries
- Example:

```php
DB::enableQueryLog();

$posts = Post::select('id', 'title')
    ->where('user_id', 1)
    ->orderBy('created_at', 'desc')
    ->get();

dd(DB::getQueryLog());
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/queries#selects)

---

## Caching

- Cache data to reduce database trips
- Several cache systems supported by Laravel, including Redis and Memcached
- Example:

```php
$posts = Cache::remember('posts.all', 3600, function () {
  return Post::all();
});

// Data is now cached for one hour
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/cache)

---

## Raw Expressions

- Allows you to write raw SQL in Eloquent queries
- Can be useful for more complex queries that are difficult to achieve with Eloquent
- Example:

```php
DB::table('users')
  ->select(DB::raw('count(*) as user_count, status'))
  ->where('status', '<>', 1)
  ->groupBy('status')
  ->get();
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/queries#raw-expressions)

---

## SubQueries

- Allows you to nest queries within other queries
- Can be useful for advanced filtering or grouping
- Example:

```php
$posts = Post::where('created_at', '>=', function($query) {
  $query->selectRaw('DATE_SUB(NOW(), INTERVAL 1 WEEK)')
    ->from('posts')
    ->where('published', true)
    ->orderBy('created_at', 'desc')
    ->skip(10)
    ->take(1);
})->get();
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/queries#subquery-selects)

---

## Inserting & Updating Records

- Use `create()` to insert a new record
- Use the `update()` method for updating records
- Example:

```php
// Creating a new record:
Post::create([
  'title' => 'My New Post',
  'body' => 'Lorem ipsum dolor sit amet.'
]);

// Updating a record:
$post = Post::find(1);
$post->update([
  'title' => 'My Updated Post',
  'body' => 'Lorem ipsum dolor sit amet.'
]);
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent#inserting-and-updating-models)

---

## Soft Deleting

- Use soft deleting to mark records as deleted instead of actually deleting them
- Can be useful for accidentally deleted records or records that need to be retrieved later
- Example:

```php
use Illuminate\Database\Eloquent\SoftDeletes;

class Post extends Model
{
  use SoftDeletes;
  
  protected $dates = ['deleted_at'];
}

// Usage:
$post = Post::find(1);
$post->delete(); // Sets deleted_at timestamp, but doesn't actually delete the record
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent#soft-deleting)

---

## Events

- Listen for events during model creation, updating, or deleting
- Can be useful for additional logging or notifications
- Example:

```php
class User extends Model
{
  protected static function boot()
  {
    parent::boot();

    static::creating(function ($user) {
        Log::info('User was created', ['user' => $user]);
    });
  }
}
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/eloquent#events)

---

## Pagination

- Use the `paginate()` method to split large result sets into pages
- Can be useful for search results or long lists
- Example:

```php
$users = User::paginate(10);
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/pagination)

---

## Query Builder

- Use the query builder for more complex queries that aren't supported in Eloquent
- Can be useful for advanced filtering or grouping
- Example:

```php
DB::table('users')
  ->select('users.*', 'departments.name as department_name')
  ->join('departments', 'users.department_id', '=', 'departments.id')
  ->where('users.active', true)
  ->orderBy('users.name', 'asc')
  ->get();
```

More information: [Laravel documentation](https://laravel.com/docs/8.x/queries)

---

## Conclusion

- Eloquent is a powerful tool for interacting with databases in Laravel
- Advanced features such as model scopes, polymorphic relations, and observers can help solve complex problems
- Use query optimization techniques, caching, and the query builder for maximum performance

---

# References

- [Laravel documentation](https://laravel.com/docs)
- [Laracasts](https://laracasts.com)
- [Symfony documentation](https://symfony.com/doc/current/index.html)