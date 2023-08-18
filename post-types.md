# Custom Post Types

The `config/post-types.php` file serves to create your own post types.

Pollen integrates the [Extended CPTs](https://github.com/johnbillion/extended-cpts) library and a post types service provider. This setup allows you to configure post types directly from a config file.

By default, there is a post type called `supplier`. You can delete it or replace it with a type that better suits your needs.

All [parameters proposed by Extended CPTs](https://github.com/johnbillion/extended-cpts/wiki/Registering-Post-Types) can be used. An example post type included in Pollen demonstrates how to change the names associated with `singular`, `plural`, and `slug`.

## Creating Multiple Custom Post Types

To create multiple custom post types, add new keys to the `post_types` array in the `config/post-types.php` file. The example below illustrates the creation of two post types: `supplier` and `book`.

```php
return [
    'supplier' => [
        'public' => true,
        'capability_type' => 'page',
        'label' => 'Supplier',
        'map_meta_cap' => true,
        'menu_position' => 5,
        'hierarchical' => false,
        'rewrite' => false,
        'query_var' => false,
        'delete_with_user' => true,
        'supports' => [
            'title',
            'revisions',
        ],
        'names' => [
            'singular' => 'Supplier',
            'plural' => 'Suppliers',
            'slug' => 'supplier',
        ],
    ],
    'book' => [
        'menu_icon' => 'dashicons-book',
        'supports' => [
            'title',
            'editor',
            'author', 'thumbnail'
        ],
        'show_in_rest' => true,
    ],
];
```

Instead of using the configuration file to define post types, you can also use the fluent PostType class. This method is more readable and provides an object-oriented approach to declaratively set post types in a "fluent" manner.

Each method has been designed to be easy to write and interpret. They are based on the naming conventions of WordPress content type parameters and the Extended CPT package. Additional methods have been introduced to enhance the understanding of their role.

Here's an example:

```php
PostType::make('form-submission', 'Form Submission', 'Form Submissions')
    ->titlePlaceholder('The custom title placeholder')
    ->private()
    ->chronological()
    ->showInAdminBar();
```

In the given example, we're creating a custom post type with the slug 'form-submission', the singular label 'Form Submission', and the plural label 'Form Submissions'. We specify that the title placeholder field contains 'The custom title placeholder', that the content type is private, ordered chronologically, and should be displayed in the admin menu bar. Expressive, isn't it? :)
