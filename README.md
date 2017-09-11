# Infusionsoft PHP SDK (beta)

[![Build Status](https://travis-ci.org/infusionsoft/infusionsoft-php.png?branch=master)](https://travis-ci.org/infusionsoft/infusionsoft-php)
[![Total Downloads](https://poser.pugx.org/infusionsoft/php-sdk/downloads.png)](https://packagist.org/packages/infusionsoft/php-sdk)
[![Latest Stable Version](https://poser.pugx.org/infusionsoft/php-sdk/v/stable.png)](https://packagist.org/packages/infusionsoft/php-sdk)

This package is currently in beta for testing.

## Install

Via Composer

``` json
{
    "require": {
        "infusionsoft/php-sdk": "~1.0"
    }
}
```

This package is compatible with PHP 5.3.3 and higher. It uses Guzzle 3.8.* as the default HTTP client which requires 5.3.3 or higher. In order to use cURL you must have the PHP cURL extension enabled.

## Usage

The client ID and secret are the key and secret for your OAuth2 application found at the [Infusionsoft Developers](https://developer.infusionsoft.com/apps/mykeys) website.

If you need an infusionsoft programmer contact [infusionsoft freelance developer](https://www.phpfreelanceprogrammer.com/infusionsoft-api.html)

```php
require_once 'vendor/autoload.php';

$infusionsoft = new \Infusionsoft\Infusionsoft(array(
	'clientId'     => 'XXXXXXXXXXXXXXXXXXXXXXXX',
	'clientSecret' => 'XXXXXXXXXX',
	'redirectUri'  => 'http://example.com/',
));

// If the serialized token is available in the session storage, we tell the SDK
// to use that token for subsequent requests.
if (isset($_SESSION['token'])) {
	$infusionsoft->setToken(unserialize($_SESSION['token']));
}

// If we are returning from Infusionsoft we need to exchange the code for an
// access token.
if (isset($_GET['code']) and !$infusionsoft->getToken()) {
	$infusionsoft->requestAccessToken($_GET['code']);
}

if ($infusionsoft->getToken()) {
	// Save the serialized token to the current session for subsequent requests
	$_SESSION['token'] = serialize($infusionsoft->getToken());

	$infusionsoft->contacts->add(array('FirstName' => 'John', 'LastName' => 'Doe'));
} else {
	echo '<a href="' . $infusionsoft->getAuthorizationUrl() . '">Click here to authorize</a>';
}
```

### Debugging

To enable debugging of requests and responses, you need to set the debug flag to try by using:

```php
$infusionsoft->setDebug(true);
```

Once enabled, logs will by default be written to an array that can be accessed by:

```php
$infusionsoft->getLogs();
```

You can utilize the powerful logging plugin built into Guzzle by using one of the available adapters. For example, to use the Monolog writer to write to a file:

```php
use Guzzle\Log\MonologLogAdapter;
use Monolog\Handler\StreamHandler;
use Monolog\Logger;

$logger = new Logger('client');
$logger->pushHandler(new StreamHandler('infusionsoft.log'));

$infusionsoft->setHttpLogAdapter(new MonologLogAdapter($logger));
```

## Testing

``` bash
$ phpunit
```


## Contributing

Please see [CONTRIBUTING](https://github.com/infusionsoft/infusionsoft-php/blob/master/CONTRIBUTING.md) for details.


## License

The MIT License (MIT). Please see [License File](https://github.com/infusionsoft/infusionsoft-php/blob/master/LICENSE) for more information.
