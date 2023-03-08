---
title: "Laravel Sessions and Cookies"
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





## Sessions and Cookies in Laravel

- Laravel provides built-in support for sessions and cookies.

- Sessions allow you to store information between HTTP requests.

- Cookies allow you to store information on the client's browser.

- In this presentation, we will learn how to work with sessions and cookies in Laravel.

---

## Storing Data in Sessions

- To store data in a session, use the `session` helper function.

- The `put` method allows us to store data in the session.

- Here is an example:

  ```php
  session()->put('key', 'value');
  ```

- This stores the value `'value'` in the session under the key `'key'`.

- We can retrieve the value using the `get` method:

  ```php
  $value = session()->get('key');
  ```

---

## Flash Data in Sessions

- Flash data is data that only persists for one request.

- We can store flash data using the `flash` method:

  ```php
  session()->flash('message', 'Success!');
  ```

- This will store the value `'Success!'` in the session under the key `'message'`, which will only be available for the next request.

- We can retrieve the value using the `get` method:

  ```php
  $message = session()->get('message');
  ```

---

## Removing Data from Sessions

- To remove a value from the session, use the `forget` method:

  ```php
  session()->forget('key');
  ```

- This will remove the value stored under the key `'key'` from the session.

- To remove all data from the session, use the `flush` method:

  ```php
  session()->flush();
  ```

- This will remove all data from the session.

---

## Cookies in Laravel

- Laravel provides an easy way to work with cookies.

- To create a cookie in Laravel, use the `cookie` function:

  ```php
  $cookie = cookie('name', 'value', 60);
  ```

- This will create a cookie named `'name'` with a value of `'value'` that expires in 60 minutes.

- Cookies can be returned as part of a response:

  ```php
  return response('Hello')->cookie($cookie);
  ```

---

## Reading Cookies

- To read a cookie in Laravel, use the `request` object:

  ```php
  $value = $request->cookie('name');
  ```

- This will retrieve the value of the cookie named `'name'`.

---

## Forgetting Cookies

- To remove a cookie in Laravel, you can create a new cookie with an expiration time in the past:

  ```php
  $cookie = cookie('name', null, -60);
  ```

- This will create a cookie named `'name'` with a value of `null` that expired 60 minutes ago.

- You can then return the response with the expired cookie:

  ```php
  return response('Goodbye')->cookie($cookie);
  ```

---

## Working with Session Variables

- You can use session variables to store user-related information.

- Here's an example of setting a session variable:

  ```php
  session()->put('user_id', $user->id);
  ```

- You can retrieve the variable using the `get` method:

  ```php
  $user_id = session()->get('user_id');
  ```

- You can also remove the variable using the `forget` method:

  ```php
  session()->forget('user_id');
  ```

---

## Protecting Sensitive Data

- Laravel provides encrypted session cookies to protect sensitive data.

- To encrypt all session data, set the `encrypt` option to `true` in the `config/session.php` file:

  ```php
  'encrypt' => true,
  ```

- With this option enabled, all session data will be encrypted before being stored in the cookie.

---

## Using Multiple Session Drivers

- You can use multiple session drivers in Laravel.

- To configure multiple session drivers, add an entry to the `config/session.php` file for each driver:

  ```php
  'connections' => [
      'redis' => [
          'driver' => 'redis',
          'connection' => 'default',
      ],

      'file' => [
          'driver' => 'file',
          'path' => storage_path('framework/sessions'),
      ],
  ],
  ```

- To use a specific session driver for a request, call the `driver` method:

  ```php
  return response('Hello')->with('greeting', 'Hello')->driver('redis');
  ```

---

## Conclusion

- Laravel provides built-in support for sessions and cookies.

- Sessions allow you to store information between HTTP requests.

- Cookies allow you to store information on the client's browser.

- Using Laravel's built-in support for sessions and cookies can make your application more secure.

---

## References

- [Laravel Sessions Documentation](https://laravel.com/docs/8.x/session)
- [Laravel Cookies Documentation](https://laravel.com/docs/8.x/requests#cookies)
- [Encrypting Session Data in Laravel](https://laravel.com/docs/8.x/session#protecting-sessions)
- [Using Multiple Session Drivers in Laravel](https://laravel.com/docs/8.x/session#using-multiple-drivers)