---
title: "Laravel Best Practices"
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


## Laravel Best Practices



- Use the latest version of Laravel to take advantage of new features and security updates
- Follow PSR standards for coding style and structure
- Use dependency injection and service providers to manage dependencies
- Utilize Laravel's built-in authentication and authorization systems
- Use Laravel's ORM, Eloquent, for database access

[Documentation on Laravel's ORM](https://laravel.com/docs/8.x/eloquent)

---

- Write concise and descriptive controller methods
- Use Route model binding to simplify controller actions
- Use middleware to apply logic to incoming requests
- Use validation to ensure input meets specific criteria
- Use Laravel collections for complex data manipulation

[Documentation on Laravel's middleware system](https://laravel.com/docs/8.x/middleware)

---

- Optimize database queries by using eager loading and query caching
- Use resourceful routing to generate consistent and predictable URLs
- Use Blade templates for efficient HTML rendering
- Use the @extends and @yield directives to create reusable view templates
- Use the @component directive to create reusable components

[Documentation on Laravel's Blade templating engine](https://laravel.com/docs/8.x/blade)

---

- Use queues to handle time-consuming tasks in the background
- Store environment-specific configuration values in environment variables
- Use Laravel's encryption features to securely store sensitive information
- Use automated testing to catch bugs and ensure code quality
- Continuously monitor and optimize application performance


[Documentation on Laravel's queue system](https://laravel.com/docs/8.x/queues)

---

## References
- Official Laravel Documentation: <https://laravel.com/docs/>
- Laravel Best Practices by Alexey Mezenin: <https://github.com/alexeymezenin/laravel-best-practices>
