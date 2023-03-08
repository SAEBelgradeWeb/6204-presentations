---
title: "Laravel Forms and Validation"
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


# Laravel Forms and Validation



## What are Laravel Forms?

- Laravel forms provide a convenient way to create HTML form elements with many built-in features.
- Forms can include fields such as checkboxes, radio buttons, select lists, text areas, and text fields.
- Forms can be used to gather data from users and send it to the server for processing.

Documentation: [https://laravel.com/docs/forms](https://laravel.com/docs/forms)

---

## Creating a Basic Form in Laravel

```php
<form method="POST" action="/your-route">
    @csrf
    <label for="name">Name:</label>
    <input type="text" id="name" name="name">
    <input type="submit" value="Submit">
</form>
```

- `@csrf` is a built-in Laravel directive that will generate a hidden input field with a CSRF token. This is used to protect against cross-site request forgery attacks.

---

## Form Validation in Laravel

- Form validation is the process of checking data submitted via a form to ensure it meets certain criteria (e.g. a field is not empty, an email address is in the correct format, etc).
- Laravel provides a simple and intuitive way to validate form data.

Documentation: [https://laravel.com/docs/validation](https://laravel.com/docs/validation)

---

## Basic Form Validation in Laravel

```php
public function store(Request $request)
{
    $request->validate([
        'name' => 'required',
        'email' => 'required|email',
    ]);

    // Store the data...
}
```

- The `validate` method will automatically redirect the user back with an error message if the validation rules fail.
- A validation error for each field will be automatically displayed in the view using the `errors` helper function.

---

## Custom Error Messages

```php
$request->validate([
    'name' => 'required|string',
    'email' => 'required|email|unique:users'
], [
    'name.required' => 'Please enter your name.',
    'email.unique' => 'This email has already been registered.'
]);
```

- Custom error messages can be defined for each validation rule.
- The message for a specific rule can be defined using the `attribute.rule` syntax.

---

## Conditional Validation

```php
$request->validate([
    'name' => 'required_if:type,admin',
    'email' => 'required_unless:type,admin',
]);
```

- Sometimes you may only want a field to be required if certain conditions are met.
- Conditional validation can be achieved using the `required_if`, `required_unless`, and other validation rules.

---

## Complex Validation

```php
$request->validate([
    'phone' => [
        'required_without_all:email,address',
        'phone:AUTO,US',
    ],
]);
```

- Complex validation rules can be created using the `array` syntax.
- In this example, the field is required if `email` and `address` are not present, and must be a valid US phone number.

Documentation: [https://laravel.com/docs/validation#rule-arrays](https://laravel.com/docs/validation#rule-arrays)

---

## Custom Validation Rules

```php
Validator::extend('phone_number', function ($attribute, $value, $parameters, $validator) {
    return preg_match('/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/', $value);
});
```

- Custom validation rules can be defined using the `Validator::extend` method.
- The method should return `true` if the validation passes, and `false` otherwise.
- The new validation rule can then be used in any form validation.

Documentation: [https://laravel.com/docs/validation#custom-validation-rules](https://laravel.com/docs/validation#custom-validation-rules)

---

## Form Requests

```php
php artisan make:request StoreBlogPost
```

- Instead of validating form data within a controller method, it can be encapsulated within a "form request" class.
- Form requests can be generated using the `php artisan make:request` command.
- These classes validate the incoming request and store the validated data on a `$validated` property.

Documentation: [https://laravel.com/docs/validation#form-request-validation](https://laravel.com/docs/validation#form-request-validation)

---

## Repopulating Form Data

```php
<input type="text" id="name" name="name" value="{{ old('name') }}">
```

- When a form is submitted and validation fails, the user is redirected back to the form page with the submitted data.
- The `old` function can be used to retrieve the value of a previously submitted field.

Documentation: [https://laravel.com/docs/requests#old-input](https://laravel.com/docs/requests#old-input)

---

## Creating Custom Error Views

```php
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```

- A custom error view can be created to display validation errors.
- The `$errors` variable is automatically created by Laravel and contains all validation errors for the current request.

Documentation: [https://laravel.com/docs/validation#working-with-error-messages](https://laravel.com/docs/validation#working-with-error-messages)

---

## Customizing Error Messages

```php
Validator::replacer('required_if', function ($message, $attribute, $rule, $parameters) {
    return str_replace(':other', $parameters[0], $message);
});
```

- Customized validation error messages can be defined using the `Validator::replacer` method.
- This allows for more fine-grained control over the error messages displayed to the user.

Documentation: [https://laravel.com/docs/validation#custom-error-messages](https://laravel.com/docs/validation#custom-error-messages)

---

## Retaining Old Input after Failed Validation

```php
return redirect('form')->withInput();
```

- After a form has failed validation, it can be helpful to retain the previously submitted data.
- The `withInput` method can be called on the redirect instance to flash the old input values to the session.

Documentation: [https://laravel.com/docs/redirects#redirecting-with-flashed-session-data](https://laravel.com/docs/redirects#redirecting-with-flashed-session-data])

---

## Testing Form Validation

```php
public function testFormValidation()
{
    $response = $this->post('/your-route', [
        'name' => '',
        'email' => 'not_an_email',
    ]);

    $response->assertSessionHasErrors(['name', 'email']);
}
```

- Unit tests can be written to verify that form validation has been implemented correctly.
- Laravel provides a simple and intuitive way to test form validation.

Documentation: [https://laravel.com/docs/testing#form-validation](https://laravel.com/docs/testing#form-validation)

---

## Conclusion

- Laravel provides a convenient and powerful way to create HTML forms and validate incoming data.
- The built-in form and validation features make it easy to create secure and user-friendly applications.
- Take advantage of these features for your next Laravel project!

---

### References

- Laravel Forms Documentation: [https://laravel.com/docs/forms](https://laravel.com/docs/forms)
- Laravel Validation Documentation: [https://laravel.com/docs/validation](https://laravel.com/docs/validation)
- Laravel Requests Documentation: [https://laravel.com/docs/requests](https://laravel.com/docs/requests)
- Laravel Redirects Documentation: [https://laravel.com/docs/redirects](https://laravel.com/docs/redirects)
- Laravel Testing Documentation: [https://laravel.com/docs/testing](https://laravel.com/docs/testing)
