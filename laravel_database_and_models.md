---
title: "Laravel Database and Models"
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




## Introduction

Laravel is one of the top PHP frameworks with powerful database and model features. In this presentation, we will explore the Laravel database and models to enhance your understanding of this framework.

---

## Eloquent ORM

Laravel uses the Eloquent ORM to interact with databases. With Eloquent, database tables are represented by models. 

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    // ...
}
```

Documentation: [https://laravel.com/docs/8.x/eloquent](https://laravel.com/docs/8.x/eloquent)

---

## CRUD Operations

Eloquent provides easy ways to execute typical CRUD operations.

```php
// Create
$user = new User;
$user->name = 'John Doe';
$user->email = 'john@example.com';
$user->save();

// Read
$users = User::all();

// Update
$user = User::find(1);
$user->name = 'Jane Doe';
$user->save();

// Delete
$user = User::find(1);
$user->delete();
```

Documentation: [https://laravel.com/docs/8.x/eloquent#basic-usage](https://laravel.com/docs/8.x/eloquent#basic-usage)

---

## Query Builder

Laravel's query builder provides an easy way to execute SQL queries without writing raw SQL.

```php
DB::table('users')
    ->where('name', 'John')
    ->orWhere('name', 'Jane')
    ->get();
```

Documentation: [https://laravel.com/docs/8.x/queries](https://laravel.com/docs/8.x/queries)

---

## Eager Loading

Eager loading is used to load relationships with the main model to reduce database queries.

```php
$users = User::with('posts')->get();
```

Documentation: [https://laravel.com/docs/8.x/eloquent-relationships#eager-loading](https://laravel.com/docs/8.x/eloquent-relationships#eager-loading)

---

## Relationships

Laravel provides various ways to define relationships between models. Here are some examples:

```php
class User extends Model
{
    public function posts()
    {
        return $this->hasMany(Post::class);
    }

    public function role()
    {
        return $this->hasOne(Role::class);
    }

    public function permissions()
    {
        return $this->belongsToMany(Permission::class);
    }
}
```

Documentation: [https://laravel.com/docs/8.x/eloquent-relationships](https://laravel.com/docs/8.x/eloquent-relationships)

---

## Polymorphic Relationships

Polymorphic relationships allow a model to belong to multiple other models on a single association.

```php
class Comment extends Model
{
    public function commentable()
    {
        return $this->morphTo();
    }
}

class Post extends Model
{
    public function comments()
    {
        return $this->morphMany(Comment::class, 'commentable');
    }
}

class Video extends Model
{
    public function comments()
    {
        return $this->morphMany(Comment::class, 'commentable');
    }
}
```

Documentation: [https://laravel.com/docs/8.x/eloquent-relationships#polymorphic-relationships](https://laravel.com/docs/8.x/eloquent-relationships#polymorphic-relationships)

---

## Scopes

Scopes are used to encapsulate commonly-used query constraints to reuse them in various parts of your application.

```php
class User extends Model
{
    public function scopeActive($query)
    {
        return $query->where('active', true);
    }
}

$activeUsers = User::active()->get();
```

Documentation: [https://laravel.com/docs/8.x/eloquent#query-scopes](https://laravel.com/docs/8.x/eloquent#query-scopes)

---

## Mutators

Mutators are used to set the value of a model attribute automatically.

```php
class User extends Model
{
    public function setPasswordAttribute($value)
    {
        $this->attributes['password'] = bcrypt($value);
    }
}
```

Documentation: [https://laravel.com/docs/8.x/eloquent-mutators](https://laravel.com/docs/8.x/eloquent-mutators)

---

## Accessors

Accessors are used to retrieve the value of a model attribute automatically.

```php
class User extends Model
{
    public function getFullNameAttribute()
    {
        return $this->first_name . ' ' . $this->last_name;
    }
}
```

Documentation: [https://laravel.com/docs/8.x/eloquent-mutators#defining-an-accessor](https://laravel.com/docs/8.x/eloquent-mutators#defining-an-accessor)

---

## Soft Deletes

Soft deletes allow you to delete records logically instead of physically. This helps to preserve data integrity while still being able to restore deleted records if needed.

```php
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Model
{
    use SoftDeletes;

    protected $dates = ['deleted_at'];
}
```

Documentation: [https://laravel.com/docs/8.x/eloquent#soft-deleting](https://laravel.com/docs/8.x/eloquent#soft-deleting)

---

## Model Factories

Laravel provides model factories to generate fake data for tests or seeders.

```php
use App\Models\User;

$factory->define(User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => bcrypt('password'),
    ];
});

$user = User::factory()->create();
```

Documentation: [https://laravel.com/docs/8.x/seeding#using-factories](https://laravel.com/docs/8.x/seeding#using-factories)

---

## Database Migrations

Laravel provides database migrations to manage database schema changes in a version-controlled and team-friendly way.

```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamp('email_verified_at')->nullable();
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
```

Documentation: [https://laravel.com/docs/8.x/migrations](https://laravel.com/docs/8.x/migrations)

---

## Reverse Migrations

Reverse migrations allow you to revert a migration with a single command.

```php
php artisan migrate:rollback
```

Documentation: [https://laravel.com/docs/8.x/migrations#rolling-back-migrations](https://laravel.com/docs/8.x/migrations#rolling-back-migrations)

---

## Database Seeding

Database seeding allows you to add test or demo data to your database.

```php
use App\Models\User;

User::factory()->count(10)->create();
```

Documentation: [https://laravel.com/docs/8.x/seeding](https://laravel.com/docs/8.x/seeding)

---

## Database Transactions

Database transactions protect your data from inconsistencies by ensuring the atomicity of queries.

```php
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
```

Documentation: [https://laravel.com/docs/8.x/database#database-transactions](https://laravel.com/docs/8.x/database#database-transactions)

---

## Query Logging

Laravel's query logging feature allows you to debug slow database queries in your application.

```php
DB::enableQueryLog();

// Your queries here

$queries = DB::getQueryLog();
```

Documentation: [https://laravel.com/docs/8.x/database#query-logging](https://laravel.com/docs/8.x/database#query-logging)

---

## Conclusion

In this presentation, we have explored some of the most powerful Laravel database and model features. Laravel makes it easy to perform CRUD operations, define relationships between models, and manage database schema changes. 

---

## References

- [https://laravel.com/docs](https://laravel.com/docs)
- [https://dev.to/banachdigital/eloquent-tips-ep-1-60ok](https://dev.to/banachdigital/eloquent-tips-ep-1-60ok)
- [https://scotch.io/starters/laravel-6-starter-kit-with-user-authentication-role-permissions-social-auth-named-routes-and-pretty-urls#feature-overview](https://scotch.io/starters/laravel-6-starter-kit-with-user-authentication-role-permissions-social-auth-named-routes-and-pretty-urls#feature-overview)
