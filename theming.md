# Creating a Theme

Creating a custom theme in this package is incredibly easy, thanks to the adapted [qirolab/laravel-themer](https://github.com/qirolab/laravel-themer) package.

## Generating a New Theme

To generate a new theme, run the following command:

```bash
php artisan theme:make
```

You'll be prompted to answer several questions to configure your theme. Optionally, you can define the theme's slug right away:

```bash
php artisan theme:make {theme-name}
```

## Update package.json

After the theme is generated, add these lines to the `package.json` located at the root of your Laravel installation:

```json
"scripts": {
  // ...
  "dev:{theme-name}": "vite --config themes/{theme-name}/vite.config.js",
  "build:{theme-name}": "vite build --config themes/{theme-name}/vite.config.js"
}
```

> **Note:** Currently, all themes rely on the Laravel installation's Node modules. Future versions will consider isolated Node modules for each theme.

## Theme Architecture

Your theme will have the following architecture:

```plaintext
theme-name/
├─ config/
│  ├─ gutenberg.php
│  ├─ images.php
│  ├─ menus.php
│  ├─ providers.php
│  ├─ sidebars.php
│  ├─ supports.php
│  ├─ templates.php
├─ css/
│  ├─ app.css
├─ js/
│  ├─ app.js
│  ├─ bootstrap.js
├─ views/
│  ├─ example.blade.php
│  ├─ layouts/
│  ├─ parts/
│  ├─ patterns/
│  ├─ home.blade.php
│  ├─ page.blade.php
│  ├─ post.blade.php
├─ index.php
├─ style.css
├─ tailwind.config.js
├─ theme.json
├─ vite.config.json
```

### File Explanation

- **config/gutenberg.php**: Defines settings related to the Gutenberg editor.
- **config/images.php**: Defines image thumbnail formats.
- **config/menus.php**: Defines WordPress menu zones.
- **config/providers.php**: Registers theme-specific service providers.
- **config/sidebars.php**: Defines WordPress widget areas.
- **config/supports.php**: Enables various WordPress theme features.
- **config/templates.php**: Allows setting of WordPress page templates.
- **css/**: Holds theme styles.
- **js/**: Contains theme-specific JavaScript files.
- **views/**: Contains Blade views, which are also available in Gutenberg editor.

### ViteJS Configuration

You may need to adapt the ViteJS configuration depending on your development environment. Refer to the Laravel Vite documentation [here](https://laravel.com/docs/10.x/vite#configuring-vite).

### Parent and Child Themes

Both are supported and work as they would with a standard WordPress theme, but with Blade for views.

### Theme Class Methods

Here are some useful methods provided by the `Theme` class:

```php
// Set the active theme
Theme::set('theme-name');

// Get the current active theme
Theme::active();

// Get the current parent theme
Theme::parent();

// Clear the active theme
Theme::clear();

// Get theme path
Theme::path($path = 'views'); // Returns the path to the active theme's views
```
