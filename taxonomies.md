# Taxonomies

Just like the implementation of post types, Pollen leverages the [Extended CPTs](https://github.com/johnbillion/extended-cpts) library, coupled with a post types service provider, to enable taxonomy configuration from a config file.

By default, the `customer_type` taxonomy is defined and linked to the `supplier` post type. You can remove this taxonomy or replace it with one that better suits your needs.

Here is an example configuration :

```php
return [
    'customer_type' => [
        'labels' => [
            'singular' => 'Customer Type',
            'plural' => 'Customer Types',
        ],
        'links' => ['supplier'],
        'meta_box' => 'radio',
        'names' => [
            'singular' => 'Customer Type',
            'plural' => 'Customer Types',
        ],
    ],
];
```

All [parameters available in Extended CPTs](https://github.com/johnbillion/extended-cpts/wiki/Registering-taxonomies) are supported.
