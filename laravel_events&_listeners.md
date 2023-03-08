---
title: "Laravel Events & Listeners"
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


## What are Laravel Events & Listeners?

- Laravel provides a simple and robust way of implementing event-driven architecture.
- Laravel's `Event` class provides methods for event registration, dispatching, and listening.
- Laravel's `Listener` interface can be implemented to handle events.





## Creating an Event

- To create a new event class, use the `php artisan event:generate` command.
- This will create a new `app/Events` folder and generate a skeleton event class.
- Edit the `$broadcastOn` property to define the channel for broadcasting the event.
- Add any additional data that should be passed with the event to the `$data` property.


---


## Dispatching an Event

- To dispatch an event, use the `event()` function, passing in an instance of the event class.
- All listeners registered for the event will receive the event and can react accordingly.
- Laravel will automatically broadcast any event that has a `$broadcastOn` property defined.


---


## Creating a Listener

- To create a new listener, use the `php artisan make:listener` command.
- This will create a new `app/Listeners` folder and generate a skeleton listener class.
- Implement the `handle()` method to define what should happen when the event is received.
- Bind the listener to the event in the `EventServiceProvider`.


---


## Binding a Listener

- Add the listener to the `$listen` property on the `EventServiceProvider`.
- The key should be the name of the event and the value should be an array of listeners.
- If the listener is a class name, it should be specified as a string.
- If the listener is a closure, it can be specified inline.


---


## Variable Listener Subscriptions

- It's possible to bind a listener with variables by passing them as arguments to the `listen()` method.
- The variables will be passed as additional parameters to the listener's `handle()` method.
- This can be useful for dynamic listener subscriptions.


---


## Queued Listeners

- Laravel also supports queued listeners.
- To queue a listener, add the `ShouldQueue` interface to the listener class.
- Laravel will handle the rest for you, queuing the listener and processing it when resources are available.


---


## Events & Models

- Laravel offers an easy way to link events to models through event broadcasting.
- By default, Laravel will automatically broadcast events that implement the `Illuminate\Broadcasting\InteractsWithSockets` trait.
- This trait provides methods to define the channel and event name, and to broadcast data with the event.


---


## Event Discovery

- By default, Laravel will discover events within the `app/Events` folder and listeners within the `app/Listeners` folder.
- To change this, modify the `$discoveredEvents` and `$discoveredListeners` properties in `EventServiceProvider.php`.
- Alternatively, you can register events and listeners manually.


---


## Manually Registering Events & Listeners

- To register events and listeners manually, use the `Event::listen()` method.
- The first argument is the name of the event.
- The second argument is the listener or an array of listeners.
- You can pass a function as the second argument to define the listeners inline.


---


## Event Discovery & Manual Registration

- You have the option to use event discovery and manual registration together.
- In this case, Laravel will discover events automatically as well as those defined via `Event::listen()`.
- This allows for a high degree of flexibility.


---


## Stopping Event Propagation

- To stop an event from propagating to other listeners, call the `stopPropagation()` method on the event object.
- This can be useful if a listener has taken care of an event and you want to ensure that other listeners don't take action as well.


---


## Prioritizing Listeners

- To define the order in which listeners should be invoked, use the `priority()` method.
- The lower the number, the higher the priority.
- Laravel will call the listeners in priority order.


---


## Subscribing to Multiple Events

- To subscribe a listener to multiple events, use the `subscribe()` method in the `EventServiceProvider`.
- The subscriber class should implement the `Subscriber` interface.
- All subscribed events and listeners should be defined within the `subscribe()` method.


---


## Event Firing & Testing

- When testing events, you can use Laravel's `Event::fake()` method to override event firing.
- After calling `fake()`, events will be queued but not dispatched.
- Use the `Event::assertDispatched()` and `Event::assertNotDispatched()` methods to test expected behavior.


---


## Event Broadcasting

- Event broadcasting is implemented using Laravel's WebSockets package.
- To broadcast an event, implement the `ShouldBroadcast` interface in the event class.
- Define the channel and payload that should be broadcast.


---


## Event Broadcasting Example

```php
class OrderShipped implements ShouldBroadcast
{
    use InteractsWithSockets;

    public function __construct($order)
    {
        $this->order = $order;
    }

    public function broadcastOn()
    {
        return new PrivateChannel('orders.' . $this->order->user_id);
    }

    public function broadcastWith()
    {
        return ['id' => $this->order->id];
    }
}
```


---


## Laravel Events & Listeners Documentation

- [Laravel Events](https://laravel.com/docs/8.x/events)
- [Laravel Listeners](https://laravel.com/docs/8.x/events#defining-listeners)
- [Laravel Event Broadcasting](https://laravel.com/docs/8.x/broadcasting)


---


## References

- [Laravel](https://laravel.com/docs/8.x)
- [Laravel WebSockets](https://github.com/beyondcode/laravel-websockets)
- [Event-Driven Architecture](https://en.wikipedia.org/wiki/Event-driven_architecture)