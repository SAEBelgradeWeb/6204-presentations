---
title: "Laravel Localization"
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


# Laravel Localization



### What is Localization?

- Localization is the process of adapting a product to meet the language, cultural, and other specific requirements of a particular country or region. 

- Laravel Localization allows you to create and maintain translations of your application text in a number of different languages.

---

### Benefits of Localization

- Wider audience: Localization provides access to a wider audience.

- Improved user experience: Developers can make an application more user-friendly through Localization.

- Increased revenue: Localization can increase revenue and brand awareness.

---

### Laravel Localization Helper methods

```php
// Getting the application's current locale:
$locale = app()->getLocale();

// Getting the translated message by key:
$message = __('welcome');

// Getting the translated message with replacement values:
$message = __('Hello :name', ['name' => 'John']);
```

---

### Language Files

- Language files are stored in the `resources/lang` directory.

- You can create a new language file using the `php artisan make:lang` command.

```php
// Create a new language file with name "messages" in "en" language
php artisan make:lang en messages
```

---

### Language Files Example

```php
// resources/lang/en/messages.php

return [
    'welcome' => 'Welcome to my Website!',
    'click_here' => 'Click :link to continue.',
    'http_error' => 'Error :code: :message',
];
```

---

### Using Language Files

```php
// resources/views/welcome.blade.php

<h1>{{ __('welcome') }}</h1>
<p>{{ __('click_here', ['link' => route('home')]) }}</p>
```

---

### Pluralization

```php
$n = 5;

// Equivalent to _n('One apple', ':count apples', $n);
echo trans_choice('{0} No apples|{1} One apple|[2,*] :count apples', $n, ['count' => $n]));

// The translation might be:
// '{0} No apples|{1} :count apple|[2,*] :count apples'
```

---

### Pluralization Example

```php
// resources/lang/en/apples.php

return [
    'apple' => '{0} No apples|{1} One apple|[2,*] :count apples',
];

// Usage:
$n = 5;
$apple = trans_choice('apples.apple', $n, ['count' => $n]));
echo $apple;
```

---

### Fallback Locale

```php
// resources/config/app.php

'fallback_locale' => 'en',
```

- If a language does not exist in the requested Locale, Laravel will use the fallback locale as a backup.

---

### Locale Selector

- Locale selector can let users choose their preferred language. 

- It is useful for settings or profile page. 

- It can be implemented using Select Dropdown or Radio Button.

---

### Custom Translation Providers

```php
// resources/config/app.php

'providers' => [
    Illuminate\Translation\TranslationServiceProvider::class,
    App\Providers\CustomTranslationServiceProvider::class,
],
```

- You can register your custom translation provider in `app.php`.

---

### Custom Translation Providers

```php
namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Translation\FileLoader;
use Illuminate\Translation\Translator;
use Illuminate\Filesystem\Filesystem;

class CustomTranslationServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->singleton('translator', function ($app) {
            $loader = new FileLoader(new Filesystem, $app->basePath().'/my_resources/lang');
            $locale = $app->getLocale();
            $trans = new Translator($loader, $locale);
            $trans->setFallback($app['config']['app.fallback_locale']);
            return $trans;
        });
    }
}
```

---

### Translation Caching

- Laravel can cache translations to speed up the application.

- Use `php artisan translation:cache` to cache the translations.

---

### Translation Validation

- You can use the `translation:find` command to validate your translations.

```bash
$ php artisan translation:find
```

- This command will search all of the translatable strings and ensure that their translations are present and contain the appropriate substitution placeholders.

---

### Conclusion

- Laravel Localization provides an easy way to localize your web applications.

- With Laravel Localization, you can easily translate application text into different languages.

- This feature ensures your app is accessible to an international audience.

---

###### References

- [Laravel Localization](https://laravel.com/docs/localization)

- [Pluralization](https://laravel.com/docs/localization#pluralization)

- [Fallback Locale](https://laravel.com/docs/localization#fallback-locales)

- [Translation Cache](https://laravel.com/docs/localization#caching-translations)

- [Translation Validation](https://laravel.com/docs/8.x/artisan#command-list)