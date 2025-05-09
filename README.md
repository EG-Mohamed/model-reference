# Laravel Model Reference

This package makes it easy to generate unique reference numbers for your Laravel models.

## Installation

1. Install the package via composer:

```bash
composer require eg-mohamed/model-reference
```

2. Publish the configuration file:

```bash
php artisan vendor:publish --tag="model-reference-config"
```

## Usage

### 1. Add the reference column to your migration

Make sure your model's table has a column to store the reference:

```php
Schema::create('orders', function (Blueprint $table) {
    $table->id();
    $table->string('reference')->unique(); // Add this column to store references
    $table->timestamps();
});
```

### 2. Use the trait in your model

```php
use MoSaid\ModelReference\Traits\HasReference;

class Order extends Model
{
    use HasReference;
}
```

That's it! Now whenever you create a new record, it will automatically generate a reference:

```php
$order = Order::create([
    'customer_id' => 1,
    'total' => 99.99,
    // No need to specify the reference; it will be generated automatically
]);

echo $order->reference; // Outputs something like "AB12CD"
```

## Customization

You can customize how references are generated by adding properties to your model:

```php
class Order extends Model
{
    use HasReference;
    
    // Change the column name (default: 'reference')
    protected $referenceColumn = 'order_number';
    
    // Set a prefix (e.g., ORD-123456)
    protected $referencePrefix = 'ORD';
    
    // Set a suffix (e.g., ORD-123456-2023)
    protected $referenceSuffix = '2023';
    
    // Change the length of the random part (default: 6)
    protected $referenceLength = 8;
    
    // Change the separator (default: '-')
    protected $referenceSeparator = '_';
    
    // Change the characters used (default: alphanumeric)
    protected $referenceCharacters = '0123456789';
}
```

With the settings above, you'd get references like: `ORD_12345678_2023`

## Global Configuration

You can also change the default settings for all models in the `config/model-reference.php` file:

```php
return [
    'column_name' => 'reference',
    'length' => 6,
    'prefix' => '',
    'suffix' => '',
    'separator' => '-',
    'characters' => '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ'
];
```
## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Mohamed Said](https://github.com/EG-Mohamed)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
