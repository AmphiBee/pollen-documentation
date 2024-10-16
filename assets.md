# Assets Management

The `Asset` class provides a streamlined way to manage the inclusion of CSS styles and JavaScript scripts in WordPress, with support for ViteJS integration and Asset Containers.

## Basic Usage

The `Asset` class simplifies adding CSS and JavaScript assets with chainable methods:

```php
$css = Asset::add('my-theme', 'css/screen.min.css')
    ->dependencies(['bootstrap'])
    ->version('1.0')
    ->media('all')
    ->toFrontend();

$js = Asset::add('theme-script', 'js/mythemescript.js')
    ->dependencies(['jquery'])
    ->version('1.0')
    ->loadInFooter()
    ->loadStrategy('defer')
    ->toFrontend()
    ->localize('myData', ['key' => 'value']);
```

## Asset Containers

Asset Containers allow you to group and manage assets with specific configurations:

### Configuring Asset Containers

You can configure Asset Containers in your service provider or configuration file:

```php
app('asset.container')->addContainer('plugin', [
    'hot_file' => public_path('plugin.hot'),
    'build_directory' => 'build/plugin',
    'manifest_path' => public_path('build/plugin/manifest.json'),
    'base_path' => 'wp-content/plugins/your-plugin'
]);
```

### Using Asset Containers

Specify a container when adding an asset:

```php
Asset::add('plugin/stylesheet', 'assets/css/app.css')
    ->container('plugin')
    ->toBackend();
```

## Theme Asset Management

The `ThemeManager` class provides a method `asset()` for managing theme-specific assets:

```php
use Pollen\Support\Facades\Theme;

// Get the URL for a theme asset
$assetUrl = Theme::asset('images/logo.png');

// Get the URL for a specific asset type
$cssUrl = Theme::asset('styles.css', 'css');
$jsUrl = Theme::asset('script.js', 'js');
$fontUrl = Theme::asset('font.woff', 'fonts');
```

The `asset()` method automatically applies the correct asset directory configuration based on the asset type.

## ViteJS Integration

To use ViteJS for asset management:

```php
Asset::add('my-vite-asset', 'path/to/asset.js')
    ->container('theme') // Optional: specify a container
    ->useVite()
    ->toFrontend();
```

## Configuration Options

The following methods are available for configuring assets:

- `add(string $handle, string $path)`: Add a new asset.
- `container(string $containerName)`: Specify the asset container to use.
- `dependencies(array $dependencies)`: Specify asset dependencies.
- `version(string $version)`: Set the asset version.
- `media(string $media)`: Set the media type for styles.
- `loadInFooter()`: Load the script in the footer.
- `loadStrategy(string $strategy)`: Set the script load strategy (e.g., 'defer').
- `toFrontend()`, `toBackend()`, `toLoginScreen()`, `toCustomizer()`, `toEditor()`: Specify where to load the asset.
- `useVite()`: Use ViteJS for this asset.

## Localizing JavaScript

Use the `localize(string $objectName, array $data)` method to localize JavaScript:

```php
Asset::add('handle', 'path/to/asset.js')
    ->localize('myData', ['key' => 'value']);
```

## Inline Content

Add inline content for CSS or JavaScript using the `inline(string $content)` method:

```php
Asset::add('my-theme', 'css/screen.min.css')
    ->inline('.panel-main { border: 1px solid blue; }');

Asset::add('typekit', 'https://use.typekit.net/fdsjhizo.js')
    ->inline('try{Typekit.load({ async: true });}catch(e){}');
```

## Default Container

A default container is automatically set for the theme. If you don't specify a container, the default will be used:

```php
Asset::add('default/script', Theme::path('assets/app.js'))
    ->toFrontend()
    ->useVite();
```

This will use the default theme container configuration.

## Non-Vite Assets

You can use Asset Containers for non-Vite assets as well:

```php
Asset::add('plugin/legacy-script', 'js/legacy.js')
    ->container('plugin')
    ->toBackend();
```

This allows for consistent asset management across your entire WordPress setup, regardless of whether you're using ViteJS or traditional asset inclusion methods.

### Important: Use of the Service Provider

It is essential to declare assets in a **Service Provider** rather than a **Hookable** class. The service provider ensures the assets are properly enqueued via the WordPress `wp_enqueue_scripts` hook. If the declaration is made within a hookable class without using a service provider, the assets might not be enqueued as expected.