Hookable Hooks
==================

- [Overview](#overview)
- [Lifecycle hooks](#lifecycle-hooks)
- [Basic hookable class](#basic-hookable-class)
- [Hookable action](#hookable-action)
- [Hookable filter](#hookable-filter)

Overview
--------

The "hookable" class is a concept created by the [Themosis Framework](https://framework.themosis.com). In lieu of plugins for adding functionalities to your WordPress application, "hookable" classes can be utilized.

Coding within an "hookable" class provides access to all APIs specified by WordPress and your application packages. By default, "hookable" classes are initiated right before mu-plugins. The class code can also be executed at a specific WordPress action or filter hook with a defined priority.

Lifecycle hooks
-------------------

Hookable classes are not automatically enlisted in the application lifecycle. Whenever a new hookable class is created, it needs to be registered in the `config/app.php` file within its `hooks` property. You will see that the standard classes are already registered when you examine the file.

Here is an example of a hookable class :

```php
<?php

namespace App\Hooks;

use Pollen\Hook\Hookable;

class MyHook extends Hookable
{
    /**
     * Augment WordPress.
     */
    public function register()
    {
        // Implement your code...
    }
}
```

Incorporate your code within the `register` method. After adding your code, you still need to register the hookable class in the `hooks` property of the `config/app.php` file like this:

```php
'hooks' => [
    App\Hooks\MyHook::class
]
```

Hookable action
---------------------------

A particular WordPress action hook can also be designated to delay the execution of an hookable class. For instance, the code within the `register` method may need to be executed only when WordPress triggers the `init` action.

To accomplish this, you just have to define a `$hook` instance property in your class and assign it the value of one of the WordPress action hooks:

```php
<?php

namespace App\Hooks;

use Pollen\Hook\Hookable;

class MyFirstHook extends Hookable
{
    public $hook = 'init';

    /**
     * Augment WordPress.
     */
    public function register()
    {
        // Code is executed only on the "init" WordPress action.
    }
}
```

Additionally, an execution priority can be established by setting the `$priority` property as follows:

```php
<?php

namespace App\Hooks;

use Pollen\Hook\Hookable;

class MyFirstHook extends Hookable
{
    public $hook = 'init';

    public $priority = 20;

    /**
     * Augment WordPress.
     */
    public function register()
    {
        // Code is executed only on the "init" WordPress action.
    }
}
```

Hookable classes are solely utilized at the application root. They serve as the entryway to WordPress APIs. When developing custom plugins or working within a theme, hookable classes are unnecessary as all WordPress APIs are already loaded.

For executing some code at a specific action, simply use the Action class or the Filter class.

Hookable filter
--------------------------------

Since filters employ the same API internally, some code can also be executed on any WordPress filters. Your filter logic should be defined within the `register()` method of the hookable class, much like an action. The difference is that the register method now receives filter parameters as arguments and requires a return value.

Here is an example of a filter on the `the_content` hook:

```php
<?php

namespace App\Hooks;

use Pollen\Hook\Hookable;

class ContentOverride extends Hookable
{
    public $hook = 'the_content';

    public function register(string $content): string
    {
        return str_replace('ugly', 'shiny', $content);
    }
}
```

Lastly, register your filter in the `config/app.php` file, the same way as an action hook class.
