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

## Parent Theme Hierarchy

Pollen supports parent themes. You can specify a parent theme in your theme's configuration, and Pollen will automatically load resources from the parent theme if they are not found in the child theme.

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

Here are some useful methods provided by the `Theme` facade:

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
