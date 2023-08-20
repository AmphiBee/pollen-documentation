# Assets

The `Asset` class provides a streamlined way to manage the inclusion of CSS styles and JavaScript scripts using WordPress' asset inclusion functions.

## Usage

The `Asset` class simplifies adding CSS styles and JavaScript scripts with chainable methods.

### Example Usage

Here's how to use the `Asset` class for adding CSS and JavaScript assets:

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

### Adding Assets

You can add CSS styles or JavaScript scripts using the `add()` method:

```php
Asset::add('handle', 'path/to/asset.css');
// or
Asset::add('handle', 'path/to/asset.js');
```

### Configuration Options

The following methods are available for configuring assets:

- `dependencies(array $dependencies)`: Specify asset dependencies.
- `version(string $version)`: Set the asset version.
- `media(string $media)`: Set the media type for styles.
- `loadInFooter()`: Load the script in the footer.
- `loadStrategy(string $strategy)`: Set the script load strategy (e.g., 'defer').
- `toFrontend()`, `toBackend()`, `toLoginScreen()`, `toCustomizer()`, `toEditor()`: Specify where to load the asset.

# ViteJS Integration

To integrate with ViteJS and load assets using the @vite/client script provided by the Laravel API, you can use the useVite() method:

```php
// Include assets using ViteJS
Asset::add('my-vite-css', 'path/to/asset.css')
    ->useVite()
    ->toFrontend();
    
Asset::add('my-vite-js', 'path/to/asset.js')
    ->useVite()
    ->toLoginScreen();
```

### Localizing JavaScript

Use the `localize(string $objectName, array $data)` method to localize JavaScript:

```php
Asset::add('handle', 'path/to/asset.js')
    ->localize('myData', ['key' => 'value']);
```

### Inline Content

You can also add inline content for CSS or JavaScript using the `inline(string $content)` method:

```php
$css = Asset::add('my-theme', 'css/screen.min.css')
    ->inline('.panel-main { border: 1px solid blue; }');

$js = Asset::add('typekit', 'https://use.typekit.net/fdsjhizo.js')
    ->inline('try{Typekit.load({ async: true });}catch(e){}');
```
