---
title: "Laravel Commands"
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
- Laravel provides a powerful Artisan command-line interface
- It allows for easy and automated management of various tasks
- However, sometimes we need to create our own custom commands

---

## Creating a Command
- Creating a custom command is straightforward
- Just create a new class that extends Laravel's `Illuminate\Console\Command` class
- Implement the `handle()` method that contains the command logic

```php
namespace App\Console\Commands;

use Illuminate\Console\Command;

class GreetUser extends Command
{
    protected $signature = 'greet {name}';
    protected $description = 'Greet the user by name.';

    public function handle()
    {
        $name = $this->argument('name');
        $this->info("Hello, $name! How are you today?");
    }
}
```

---

## Registering a Command
- To register the command, add it to the `commands` array in `app/Console/Kernel.php`
- Alternatively, you can use the `load()` method in the same file to auto-discover commands in a directory

```php
protected $commands = [
    \App\Console\Commands\GreetUser::class,
];
```

---

## Command Signature
- The `$signature` property specifies the command name and any parameters or options
- Commands are called using `php artisan` followed by the command name and any arguments

```php
protected $signature = 'greet {name} {--loud}';
```

```bash
php artisan greet John         # outputs "Hello, John!"
php artisan greet John --loud  # outputs "HELLO, JOHN!"
```

---

## Command Descriptions
- The `$description` property provides a brief explanation of what the command does
- It is displayed when using the `php artisan list` command to show all available commands

```php
protected $description = 'Greet the user by name.';
```

---

## Output
- Laravel provides several helper methods to output text to the console
- `info()`: standard output in green text
- `comment()`: comment output in yellow text
- `error()`: error output in red text
- `line()`: plain text output

```php
$this->info('This is some information');
$this->comment('This is a comment');
$this->error('This is an error');
$this->line('Just plain text');
```

---

## Input
- Laravel can gather input from the user in various ways
- `argument()`: retrieve an argument passed to the command
- `option()`: retrieve an option passed to the command
- `ask()`: prompt the user to enter a value
- `confirm()`: prompt the user to confirm a choice

```php
$name = $this->argument('name');
if ($this->option('loud')) {
    $this->info(strtoupper("Hello, $name!"));
} else {
    $this->info("Hello, $name!");
}
```

---

## Tables
- Laravel can output data in a nicely formatted table
- Use the `table()` method and pass in an array of data

```php
$data = [
    ['Name', 'Age'],
    ['John', 20],
    ['Jane', 25],
];
$this->table(['Name', 'Age'], $data);
```

---

## Progress Bars
- Laravel can display a progress bar for long-running tasks
- Use the `output` object to create a `ProgressBar` instance

```php
$bar = $this->output->createProgressBar(10);
$bar->start();
foreach (range(1, 10) as $i) {
    sleep(1);
    $bar->advance();
}
$bar->finish();
```

---

## Additional Options
- Laravel commands can have additional options for customization
- `$name`: Specify the command name
- `$description`: Set the command description
- `$hidden`: Hide the command from the list of available commands
- `$help`: Provide help text for the command

---

## Command Events
- Laravel provides a few events that are fired during command execution
- `Illuminate\Console\Events\CommandStarting`: Fired when the command is starting
- `Illuminate\Console\Events\CommandFinished`: Fired when the command has finished
- `Illuminate\Console\Events\CommandErrored`: Fired when the command throws an exception

---

## Conclusion
- Custom Laravel commands are easy to create and provide a powerful way to automate tasks
- They can be registered and used just like built-in Artisan commands
- Laravel provides many helpful helpers for handling input and output

---

## References
- [Laravel Documentation: Artisan Console](https://laravel.com/docs/8.x/artisan)
- [Laravel Documentation: Writing Commands](https://laravel.com/docs/8.x/artisan#writing-commands)

---

## References (Cont'd)
- [Laravel Documentation: Command Events](https://laravel.com/docs/8.x/artisan#command-events)
- [Laravel Documentation: Outputting Tables](https://laravel.com/docs/8.x/artisan#outputting-tables)
- [Laravel Documentation: Outputting Progress Bars](https://laravel.com/docs/8.x/artisan#outputting-progress-bars)

---

## References (Cont'd)
- [Laravel Documentation: Customizing the Artisan Console](https://laravel.com/docs/8.x/artisan#customizing-the-artisan-console)
- [Laravel Documentation: Testing Artisan Commands](https://laravel.com/docs/8.x/testing#artisan-command-tests)
- [Laravel Code Examples: Custom Commands](https://github.com/laravelio/awesome-laravel#custom-commands)

---

## References (Cont'd)
- [Laravel News: Defining Laravel Console Commands Programmatically](https://laravel-news.com/defining-console-commands-programmatically)
- [Laravel News: Laravel Artisan Custom Commands](https://laravel-news.com/laravel-artisan-custom-commands)
- [Laracasts: The Art of Command Line](https://laracasts.com/series/guest-spotlights/episodes/1)
