# Ajax

The `Ajax` facade simplifies the management of AJAX calls in your Laravel application by providing a simple and configurable abstraction.

The `Ajax` class allows you to manage AJAX calls using chainable configuration methods.

### Example Usage

Here's an example of using the `Ajax` class with the fluent syntax:

```php
Ajax::listen('my_action', function () {
    // AJAX handling logic here
})->forLoggedUsers();
```

### Instance Creation and Configuration

To start, you can use the `listen()` method of the `Ajax` Facade to create an instance and configure the action and associated callback:

```php
Ajax::listen('my_action', function () {
    // AJAX handling logic here
});
```

### Configuration for Target Users

Use the `forLoggedUsers()` and `forGuestUsers()` methods to specify the target users for the AJAX call:

```php
Ajax::listen('my_action', function () {
    // AJAX handling logic here
})->forLoggedUsers();
// or
Ajax::listen('my_action', function () {
    // AJAX handling logic here
})->forGuestUsers();
```

### Complete Example

Here's a complete example of using the `Ajax` class:

```php
$response = Ajax::listen('my_action', function () {
    // AJAX handling logic here
})->forLoggedUsers()
->execute();
```

The above example configures an action, a callback, and target users, and then executes the AJAX call. The server response is stored in the `$response` variable.
