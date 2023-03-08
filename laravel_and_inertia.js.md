---
title: "Laravel and Inertia.js"
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


## What is Laravel?

Laravel is a popular PHP framework that is built with the goal of making web development easier and more efficient. It accomplishes this through a robust set of tools and features that streamline the process of creating everything from small, simple sites to large, complex web applications.




## What is Inertia.js?

Inertia.js is a relatively new component of the Laravel ecosystem that provides a way to build single-page applications (SPAs) using a combination of Laravel and Vue.js. With Inertia, you can create dynamic, responsive web experiences that are lightning fast and provide a more engaging user experience than traditional multi-page sites.


---

## Why use Laravel and Inertia.js together?

Laravel and Inertia.js are both designed to be flexible and easy to use, so they work very well together. When combined, they allow developers to create powerful, dynamic web applications in a way that is intuitive, fast, and scalable. The result is a more streamlined development process that leads to higher-quality, more engaging web experiences for users.


---

## Getting started with Laravel and Inertia.js

To begin working with Laravel and Inertia.js, you should first have a basic understanding of both technologies. Here are a few resources to help you get started:

- [Laravel Documentation](https://laravel.com/docs)
- [Inertia.js Documentation](https://inertiajs.com/docs)
- [Vue.js Documentation](https://vuejs.org/v2/guide/)


---

## Creating a new Laravel project

To create a new Laravel project, use the Laravel CLI:

```bash
laravel new myproject
```

This will create a new Laravel project in a directory called "myproject". From there, you can start building your application using any of the tools and features provided by Laravel, such as routing, controllers, models, and migrations.


---

## Adding Inertia.js to your Laravel project

To add Inertia.js to your Laravel project, you can use Composer:

```bash
composer require inertiajs/inertia-laravel
```

This will install the necessary dependencies and configure your Laravel project to work with Inertia.js. From there, you can start building your Inertia-powered SPA.


---

## Creating Inertia.js components

Inertia.js components are Vue.js components that are designed to work with Laravel's routing, controllers, and views. To create an Inertia.js component, simply create a new Vue.js component with a template that contains your markup and a script block that declares your component using `Inertia.page()`:

```html
<template>
    <div>
        <h1>Hello, world!</h1>
        <p>This is an Inertia.js component.</p>
    </div>
</template>

<script>
    import { Inertia } from '@inertiajs/inertia';

    export default {
        mounted() {
            Inertia.page(this);
        }
    }
</script>
```


---

## Rendering Inertia.js components

To render an Inertia.js component, you simply return it from your Laravel controller:

```php
<?php

namespace App\Http\Controllers;

use Inertia\Inertia;

class HomeController extends Controller
{
    public function index()
    {
        return Inertia::render('Home');
    }
}
```

This will render the "Home" component and send it to the client as a JavaScript object that can be used to update the DOM.


---

## Passing data to Inertia.js components

You can pass data to your Inertia.js components using Laravel's compact() function:

```php
<?php

namespace App\Http\Controllers;

use Inertia\Inertia;

class HomeController extends Controller
{
    public function index()
    {
        $data = [
            'pageTitle' => 'Home',
            'message' => 'Welcome to my Inertia.js app!'
        ];

        return Inertia::render('Home', compact('data'));
    }
}
```

This will make the `$data` variable available to your "Home" component as a JavaScript object.


---

## Updating the page title with Inertia.js

To update the page title when a user navigates to a new page in your Inertia.js app, you can add a `title` property to your `momentum.js` file:

```javascript
// resources/js/momentum.js

import { InertiaApp } from '@inertiajs/inertia-vue';
import { InertiaForm } from '@inertiajs/inertia-vue';
import Vue from 'vue/dist/vue';

Vue.use(InertiaApp);
Vue.use(InertiaForm);

const app = document.getElementById('app');

new Vue({
    metaInfo: {
        titleTemplate: title => (title ? `${title} - My App` : 'My App'),
    },

    render: h => h(InertiaApp, {
        props: {
            initialPage: JSON.parse(app.dataset.page),
            resolveComponent: name => import(`./Pages/${name}`).then(module => module.default),
        },
    }),
}).$mount(app);
```


---

## Handling form submissions with Laravel and Inertia.js

To handle form submissions with Inertia.js, you can use Laravel's built-in form validation tools and return a response that includes a `redirect` property:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;
use Inertia\Inertia;

class ContactController extends Controller
{
    public function store(Request $request)
    {
        $rules = [
            'name' => 'required',
            'email' => 'required|email',
            'message' => 'required',
        ];

        $validator = Validator::make($request->all(), $rules);

        if ($validator->fails()) {
            return back()->withErrors($validator)->withInput();
        }

        // Store the message in the database...

        return Inertia::location('/thanks');
    }
}
```

This will submit the form using AJAX and redirect the user to the "/thanks" page.


---

## Displaying validation errors with Inertia.js

To display form validation errors in your Inertia.js app, you can use Laravel's `$errors` variable, which is automatically included in your component data:

```html
<template>
    <form @submit.prevent="handleSubmit">
        <div>
            <label>Name:</label>
            <input type="text" v-model="form.name">
            <span v-if="$errors.name" class="error">{{ $errors.name[0] }}</span>
        </div>

        <div>
            <label>Email:</label>
            <input type="email" v-model="form.email">
            <span v-if="$errors.email" class="error">{{ $errors.email[0] }}</span>
        </div>

        <div>
            <label>Message:</label>
            <textarea v-model="form.message"></textarea>
            <span v-if="$errors.message" class="error">{{ $errors.message[0] }}</span>
        </div>

        <button type="submit">Submit</button>
    </form>
</template>

<script>
    import { Inertia } from '@inertiajs/inertia';

    export default {
        data() {
            return {
                form: {
                    name: '',
                    email: '',
                    message: '',
                },
            };
        },

        methods: {
            async handleSubmit() {
                await Inertia.post('/contact', this.form);
            },
        },
    };
</script>
```


---

## Handling authentication with Laravel and Inertia.js

To handle authentication with Inertia.js, you can use Laravel's built-in authentication tools, such as the `Auth` middleware and the `Auth::user()` function:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Auth;
use Inertia\Inertia;

class DashboardController extends Controller
{
    public function index()
    {
        $user = Auth::user();

        return Inertia::render('Dashboard', [
            'user' => $user,
        ]);
    }
}
```

This will check if the user is authenticated and send their information to the "Dashboard" component as a JavaScript object.


---

## Customizing the Inertia.js page component

To customize the Inertia.js page component that is created by default, you can use Laravel's view inheritance system to create a new layout file:

```html
<!-- resources/views/layouts/app.blade.php -->

<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    @yield('meta')

    <title>@yield('title') - My App</title>
</head>
<body>
    @yield('content')
</body>
</html>
```


---

## Using shared data in Inertia.js components

To share data between Inertia.js components, you can use Laravel's `AppServiceProvider` to define a shared data object:

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;
use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Request;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        View::share('currentUser', Auth::user());
        View::share('currentUrl', Request::path());
    }
}
```

This will make the `$currentUser` and `$currentUrl` variables available to all of your Inertia.js components.


---

## Conclusion

Laravel and Inertia.js are powerful tools that can be used to create dynamic, engaging web experiences that are fast, responsive, and easy to use. By leveraging the strengths of both technologies, you can create web applications that are flexible, scalable, and attractive to users.


---

## References

- [Laravel Documentation](https://laravel.com/docs)
- [Inertia.js Documentation](https://inertiajs.com/docs)
- [Vue.js Documentation](https://vuejs.org/v2/guide/)
- [Laravel API Documentation](https://laravel.com/api)
- [Inertia.js API Documentation](https://inertiajs.com/api)
- [Vue.js API Documentation](https://vuejs.org/v2/api/)