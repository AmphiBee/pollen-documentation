# Creating a Theme

## Introduction

The Pollen framework offers a robust and flexible system for creating and managing WordPress themes. This guide will help you understand how to create, customize, and use themes with Pollen.

## Creating a New Theme

## Generating a New Theme

To generate a new theme, run the following command:

```bash
php artisan theme:make
```

You'll be prompted to answer several questions to configure your theme. Optionally, you can define the theme's slug right away:

```bash
php artisan theme:make {theme-name}
```

This command creates a new theme with the necessary folder structure and base files.

### Theme Structure

A typical Pollen theme has the following structure:

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
├─ assets/
│  └─ css/
│      └─ app.css
│  └─ fonts/
│  └─ images/
│  └─ js/
│      `└─ `bootstrap.js
│  └─ app.js
├─ views/
│  ├─ example.blade.php
│  ├─ layouts/
│  ├─ parts/
│  ├─ patterns/
│  ├─ home.blade.php
│  ├─ page.blade.php
│  └─ post.blade.php
├─ favicon.png
├─ index.php
├─ package.json
├─ style.css
├─ tailwind.config.js
├─ theme.json
├─ vite.config.json
└─ vite.config.js
```

## Vite Configuration

The `vite.config.js` file is automatically generated and configured for your theme. It includes:

- Automatic theme name detection
- Configuration for development and production
- Integration with the Laravel Vite plugin

```javascript
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";
import path from 'path';

const isDevelopment = !!process.env.DDEV_PRIMARY_URL;
const port = 5173;
const publicDirectory = path.resolve(__dirname, "../../public");
const themeName = path.basename(__dirname);

// ... (detailed configuration)

export default defineConfig({
    plugins: [
        laravel(getThemeConfig()),
        // ... (other plugins)
    ],
    ...getDevServerConfig()
});
```

## TailwindCSS Integration

Each Pollen theme is pre-configured with TailwindCSS. The `package.json` file includes the necessary dependencies:

```json
{
    "devDependencies": {
        "@tailwindcss/forms": "^0.5.6",
        "@tailwindcss/typography": "^0.5.9",
        "tailwindcss": "^3.3.3",
        // ... (other dependencies)
    },
    "dependencies": {
        "@tailwindcss/aspect-ratio": "^0.4.2"
    }
}
```

To use TailwindCSS in your theme, make sure to include the necessary directives in your main CSS file.

## Theme Management

Pollen's `ThemeManager` offers several useful methods for managing themes:

- `Theme::load($themeName)`: Loads a specific theme
- `Theme::getAvailableThemes()`: Retrieves the list of available themes
- `Theme::active()`: Returns the name of the active theme
- `Theme::parent()`: Returns the name of the parent theme (if it's a child theme)
- `Theme::path($path)`: Generates the full path to a file in the active theme
- `Theme::asset($path, $assetType = '')`: Retrieves the URL for a theme asset

### Using Theme Assets

The `asset()` method allows you to easily reference theme assets:

```php
use Pollen\Support\Facades\Theme;

// Get the URL for an image in the theme
$logoUrl = Theme::asset('logo.png', 'images');

// Get the URL for a CSS file
$styleUrl = Theme::asset('app.css', 'css');

// Get the URL for a JavaScript file
$scriptUrl = Theme::asset('app.js', 'js');
```

This method uses the asset directory configuration specified in your theme's `config.php` file.

## Asset Directory Configuration

In your theme's `config.php` file, you can specify the directory structure for different asset types:

```php
return [
    // ... autres configurations ...
    'asset_dir' => [
        'root' => 'assets',
        'images' => 'images',
        'fonts' => 'fonts',
        'css' => 'css',
        'js' => 'js',
    ],
];
```

This configuration is used by the `Theme::asset()` method to locate assets correctly.

## Localization

Language files should be placed in the `lang/` folder of your theme. Pollen will load them automatically.

## Theme Development

Commands needs to be run inside the theme folder.

1. Install dependencies:
   ```bash
   yarn # or 'npm install'
   ```

2. For development, use:
   ```bash
   yarn dev # or 'npm run dev'
   ```

3. For production, build the assets with:
   ```bash
   yarn build # or 'npm run build'
   ```

## Best Practices

- Use the provided folder structure to organize your code
- Take advantage of TailwindCSS for rapid and consistent CSS development
- Use the `ThemeManager` methods for efficient theme management
- Consider parent theme compatibility when developing

## Conclusion

The Pollen framework offers a powerful and flexible theme system, integrating modern tools like Vite and TailwindCSS. By following these guidelines, you can create robust and maintainable WordPress themes.

## Asset Management

### Asset Directory Configuration

In your theme's `config.php` file, you can specify the directory structure for different asset types:

```php
return [
    // ... autres configurations ...
    'asset_dir' => [
        'root' => 'assets',
        'images' => 'images',
        'fonts' => 'fonts',
        'css' => 'css',
        'js' => 'js',
    ],
];
```

This configuration is used by the `Theme::asset()` method to locate assets correctly.

### Using Theme Assets

The `asset()` method allows you to easily reference theme assets:

```php
use Pollen\Support\Facades\Theme;

// Get the URL for an image in the theme
$logoUrl = Theme::asset('logo.png', 'images');

// Get the URL for a CSS file
$styleUrl = Theme::asset('app.css', 'css');

// Get the URL for a JavaScript file
$scriptUrl = Theme::asset('app.js', 'js');
```

### Vite Macros for Theme Assets

The `ThemeServiceProvider` defines several Vite macros that utilize the `asset_dir` configuration to easily access theme assets. These macros provide a convenient way to reference different types of assets in your theme:

#### Available Macros

- `Vite::image()`: For referencing image assets
- `Vite::font()`: For referencing font assets
- `Vite::css()`: For referencing CSS assets
- `Vite::js()`: For referencing JavaScript assets

#### Usage

You can use these macros in your Blade templates or PHP code to generate URLs for your theme assets:

```php
// In a Blade template
<img src="{{ Vite::image('logo.png') }}" alt="Logo">
<link rel="stylesheet" href="{{ Vite::css('app.css') }}">
<script src="{{ Vite::js('app.js') }}"></script>

// In PHP code
$logoUrl = Vite::image('logo.png');
$fontUrl = Vite::font('myfont.woff2');
$styleUrl = Vite::css('styles.css');
$scriptUrl = Vite::js('script.js');
```

These macros internally use the `ThemeManager::asset()` method, applying the correct asset type and directory structure based on your theme's `asset_dir` configuration.

#### Benefits

- Simplified asset referencing in your theme
- Automatic application of the correct asset directory based on asset type
- Seamless integration with Vite and your theme's asset structure

By using these macros, you ensure that your asset references are consistent with your theme's configuration and take advantage of Vite's asset handling capabilities.
