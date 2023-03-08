---
title: "Testing in Laravel"
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



## Why Test?

- Ensure that your code does what it's supposed to do.
- Catch errors before they make it to production.
- Decrease the amount of time spent debugging.
- Foster confidence and trust in the codebase.

---

## PHPUnit

- PHPUnit is the testing framework that ships with Laravel.
- It is an instance of the xUnit architecture for writing automated tests.
- You can run PHPUnit tests by running `php artisan test`.

---

## Assertions

- Assertions are used to verify that your code is functioning as expected.
- You can use various built-in assertions.  

```php
public function testBasicExample()
{
    $this->assertTrue(true);
    $this->assertEquals(1, 1);
    $this->assertEmpty([]);
}
```

---

## Writing a Test

- Create a TestCase by running `php artisan make:test`.
- Write one or more tests inside that class.  

```php
use App\Models\User;
use Tests\TestCase;

class UserTest extends TestCase
{
    public function testUserCanBeCreated(): void
    {
        $user = User::factory()->create([
            'name' => 'John Doe',
        ]);

        $this->assertEquals('John Doe', $user->name);
    }
}
```

---

## Test Driven Development (TDD)

- TDD is a methodology where you write tests before writing the code.  

```php
public function testUserCanBeCreated(): void
{
    $user = User::factory()->create([
        'name' => 'John Doe',
    ]);

    $this->assertEquals('John Doe', $user->name);
}
```

- The code is written and modified until the tests pass.

---

## Factories

- Factories help you create model instances.
- You can write a factory for a model by running `php artisan make:factory`.

```php
use App\Models\User;
use Illuminate\Database\Eloquent\Factories\Factory;

class UserFactory extends Factory
{
    protected $model = User::class;

    public function definition(): array
    {
        return [
            'name' => $this->faker->name,
            'email' => $this->faker->unique()->safeEmail,
            'password' => bcrypt($this->faker->password),
        ];
    }
}
```

---

## Mocking

- Mocking allows you to create fake objects.
- You should mock objects that have external dependencies.  

```php
use App\Services\StripeService;
use Illuminate\Foundation\Testing\WithoutMiddleware;
use Illuminate\Foundation\Testing\DatabaseTransactions;

class UserControllerTest extends TestCase
{
    use DatabaseTransactions, WithoutMiddleware;

    public function testUserController()
    {
        $mock = $this->mock(StripeService::class);
        $mock
            ->shouldReceive('makePayment')
            ->once()
            ->andReturn('Payment successful.');

        $response = $this->post('/users/1/charge', ['amount' => 100]);
        $response->assertStatus(200);
    }
}
```

---

## HTTP Testing

- HTTP testing allows you to test endpoints.
- You can send GET, POST, PUT, PATCH, DELETE, OPTIONS, or HEAD requests.

```php
public function testUsersCanBeListed(): void
{
    $user = User::factory()->create();
    $response = $this->get('/users');
    $response
        ->assertStatus(200)
        ->assertSee($user->name);
}
```

---

## Browser Testing

- Laravel Dusk is the framework's browser automation tool.
- It allows you to interact with your application as if you were clicking and typing.

```php
public function testLoginPage()
{
    $this->browse(function ($browser) {
        $browser->visit('/login')
                ->type('email', 'test@test.com')
                ->type('password', 'password')
                ->press('Login')
                ->assertPathIs('/dashboard');
    });
}
```

---

## Syncing Data

- You can use the `refreshDatabase` trait to clean the database in between each test.

```php
use Illuminate\Foundation\Testing\RefreshDatabase;
use Illuminate\Foundation\Testing\WithFaker;
use Tests\TestCase;

class JobTest extends TestCase
{
    use RefreshDatabase, WithFaker;

    public function testJobCanBeCreated(): void
    {
        $job = Job::factory()->create([
            'title' => $this->faker->jobTitle,
        ]);

        $this->assertInstanceOf(Job::class, $job);
    }
}
```

---

## Testing Emails

- You can use the `Mail::fake` method to fake email sending.
- You can then assert that an email was sent.

```php
public function testEmailIsSentOnRegistration(): void
{
    Mail::fake();
    $user = User::factory()->create();
    $response = $this->post('/register', [
        'name' => 'Test User',
        'email' => 'test@test.com',
        'password' => 'password',
        'password_confirmation' => 'password',
    ]);

    Mail::assertSent(WelcomeEmail::class);
}
```

---

## Code Coverage

- Code coverage measures how much of your code is being tested.
- You can use PHPUnit's `--coverage` option to generate a report.  

```bash
vendor/bin/phpunit --coverage-html=coverage
```

---

## Continuous Integration

- Continuous integration ensures that your code is being tested regularly.
- Configure a CI service to run your tests automatically.
- Popular CI services include Travis CI and CircleCI.

---

## Useful Resources

- [Laravel Documentation](https://laravel.com/docs/8.x/testing)
- [PHPUnit Documentation](https://phpunit.readthedocs.io/)
- [Mockery Documentation](https://github.com/mockery/mockery)
- [Dusk Documentation](https://laravel.com/docs/8.x/dusk)
- [Code coverage tools](https://github.com/codecov/example-php#available-tools)

---

## References

- [The Practical Test Pyramid](http://martinfowler.com/articles/practical-test-pyramid.html)
- [The Three Amigos](https://www.agilealliance.org/glossary/three-amigos)
- [Test Driven Development by Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
