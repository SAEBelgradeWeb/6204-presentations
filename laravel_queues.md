---
title: "Laravel Queues"
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

- Asynchronous processing is important to improve the performance of an application
- Queuing is a method to execute time-consuming tasks later when the server is not busy
- Laravel Queues provides an easy and simple way to implement queuing in your application

---

## Configuration

- Queues can be configured in the `config/queue.php` file
- Choose the queue driver that suits your application requirements (Sync, Database, Redis, Beanstalkd, Amazon SQS, etc.)
- Example configuration for the `database` driver:

```php
'default' => env('QUEUE_CONNECTION', 'database'),

'connections' => [

    ...

    'database' => [
        'driver' => 'database',
        'table' => 'jobs',
        'queue' => 'default',
        'retry_after' => 90,
    ],

    ...
]
```

---

## Creating Jobs

- Jobs are created using the `make:job` Artisan command
- Example command to create a job:

```bash
php artisan make:job MyJob
```

- Jobs are stored in the `app/Jobs` directory by default
- Example of a simple job:

```php
<?php

namespace App\Jobs;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;

class MyJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    /**
     * Execute the job.
     *
     * @return void
     */
    public function handle()
    {
        // Do some time-consuming task here
    }
}
```

---

## Dispatching Jobs

- Jobs can be dispatched using the `dispatch` function
- Example:

```php
MyJob::dispatch();
```

- Jobs can be dispatched with a delay using the `delay` method
- Example:

```php
MyJob::dispatch()->delay(now()->addMinutes(5));
```

- Jobs can be dispatched to a specific queue using the `onQueue` method
- Example:

```php
MyJob::dispatch()->onQueue('my-queue');
```

---

## Running Workers

- Workers are processes that run continuously to process queued jobs
- Workers can be started using the `queue:work` Artisan command
- Example command to start a worker:

```bash
php artisan queue:work
```

- A supervisor can be used to manage your workers and ensure they are always running

---

## Monitoring Queues

- Laravel provides several tools to monitor your queues and jobs
- `queue:listen` command can be used to monitor a queue

```bash
php artisan queue:listen
```

- The `horizon` package can be used to monitor and manage queues in a more advanced way -> link to the documentation: https://laravel.com/docs/queues#monitoring-queue-health-with-horizon

---

## Retrying Failed Jobs

- Failed jobs can be automatically retried based on their configuration
- The `retryAfter` property on a job determines how long it should wait before retrying
- The `tries` property determines how many times a job should be retried
- Failed jobs can also be manually retried by calling the `retry` method on the job instance

---

## Creating Custom Queue Drivers

- Laravel provides a simple way to create custom queue drivers
- Custom drivers can be included as a package, or in the `App\Queues` directory
- Custom drivers should implement the `Illuminate\Contracts\Queue\Queue` interface
- Example custom driver:

```php
<?php

namespace App\Queue;

use Illuminate\Contracts\Queue\Queue as QueueContract;

class MyCustomQueue implements QueueContract
{
    // Implement the queue methods here
}
```

---

## Scaling Workers

- Scaling workers is important to handle a large number of jobs and reduce processing time
- Increasing the number of workers will result in more jobs being processed concurrently
- Use a supervisor to manage multiple workers and control the queue processing

---

## Conclusion

- Queues are an important method for asynchronous processing in Laravel applications
- Laravel Queues provides a simple and easy way to implement queuing
- Custom queue drivers can be created for specific use cases

---

## References

- Laravel Queues Documentation: <https://laravel.com/docs/queues>
- Laravel Horizon Documentation: <https://laravel.com/docs/horizon>
- Laravel Supervisor Configuration: <https://laravel.com/docs/queues#supervisor-configuration>
- Laravel Queue Drivers Documentation: <https://laravel.com/docs/queues#driver-prerequisites>
