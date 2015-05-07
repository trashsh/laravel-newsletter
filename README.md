# Manage newsletters in Laravel 5
[![Latest Version](https://img.shields.io/github/release/freekmurze/laravel-newsletter.svg?style=flat-square)](https://github.com/freekmurze/laravel-newsletter/releases)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/freekmurze/laravel-newsletter/master.svg?style=flat-square)](https://travis-ci.org/freekmurze/laravel-newsletter)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/10993a65-449a-488a-886c-f810b9950070/mini.png)](https://insight.sensiolabs.com/projects/10993a65-449a-488a-886c-f810b9950070)
[![Quality Score](https://img.shields.io/scrutinizer/g/freekmurze/laravel-newsletter.svg?style=flat-square)](https://scrutinizer-ci.com/g/freekmurze/laravel-newsletter)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-newsletter.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-newsletter)

This package provides an easy way to integrate email marketing services with Laravel 5. Currently the package only supports MailChimp. In the future more services may get added.

## Installation

You can install this package via composer using:

```bash
composer require spatie/laravel-newsletter
```

You must also install this service provider.

```php

// config/app.php

'providers' => [
    ...
    'Spatie\Newsletter\NewsletterServiceProvider',
    ...
];
```

If you want to make use of the facade you must install it as well.

```php
// config/app.php
'aliases' => [
    ...
    'Newsletter' => 'Spatie\Newsletter\NewsletterFacade',
];
```

To publish the config file to ``app/config/laravel-newsletter.php`` run:

```bash
php artisan vendor:publish --provider="Spatie\Newsletter\NewsletterServiceProvider"
```

This wil publish a file `laravel-newsletter` in your config directory with the following contents: 
```php
return [

    'mailChimp' => [
        /*
         * The api key of your MailChimp account. You can find yours here:
         * https://us10.admin.mailchimp.com/account/api-key-popup/
         */
        'apiKey' => getenv('MAILCHIMP_APIKEY'),

        /*
         * Here you can define properties of the lists you want to
         * send campaigns.
         */
        'lists' => [
            /*
             * This key is used to identify this list. It can be used
             * in the various methods provided by this package.
             *
             * You can set it to any string you want and you can add
             * as many lists as you want.
             */
            'subscribers' => [
                /*
                 * A mail chimp list id. Check the mailchimp docs if you don't know
                 * how to get this value:
                 * http://kb.mailchimp.com/lists/managing-subscribers/find-your-list-id
                 */
                'id' => '',

                /*
                 * These values will be used when creating a new campaign.
                 */
                'fromEmail' => '',
                'fromName' => '',
                'toName' => '',
            ],
        ],
    ],
];

```

## Usage

After you've installed the package and filled in the values in the config-file working with this package will be a breeze.

Subscribing an e-mailadress can be done like this:

```php
Newsletter::subscribe('rincewind@discworld.com');
```

Let's unsubcribe someone:

```php
Newsletter::unsubscribe('the.luggage@discworld.com');
```


This is how you create a campaign:

```php
$subject = 'The Truth newsletter';
$contents = '<h1>Big news</h1>The world is carried by four elephants on a turtle!';

Newsletter::createCampaign($subject, $contents);
```

If you have multiple lists defined in the config file you must pass the name of the list an extra parameter:

```php
Newsletter::subscribe('havelock.vetinari@discworld.com', 'mySecondList');
Newsletter::unsubscribe('sam.vimes@discworld.com', 'mySecondList');

Newsletter::createCampaign($subject, $contents, 'mySecondList);
```

If you need more functionality you get an instance of the underlying service api with:

```
$api = Newsletter::getApi();
```

As this package currently only supports MailChimp this method will always return an instance of [the MailChimp API](https://bitbucket.org/mailchimp/mailchimp-api-php).


## Testing

Run the tests with:
```bash
vendor/bin/phpunit
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email [freek@spatie.be](mailto:freek@spatie.be) instead of using the issue tracker.

## Credits

- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

This package was inspired by the [Bulk Email Notifications series on Laracasts](https://laracasts.com/lessons/bulk-email-notifications-part-1).

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

