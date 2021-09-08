azure-queue-laravel
=============

[![Build Status](https://travis-ci.org/squigg/azure-queue-laravel.png?branch=master)](https://travis-ci.org/squigg/azure-queue-laravel)
[![Coverage Status](https://coveralls.io/repos/github/squigg/azure-queue-laravel/badge.svg?branch=master)](https://coveralls.io/github/squigg/azure-queue-laravel?branch=master)

[![Latest Stable Version](https://poser.pugx.org/squigg/azure-queue-laravel/v/stable.png)](https://packagist.org/packages/squigg/azure-queue-laravel)
[![Total Downloads](https://poser.pugx.org/squigg/azure-queue-laravel/downloads.png)](https://packagist.org/packages/squigg/azure-queue-laravel)

PHP Laravel Queue Driver package to support Microsoft Azure Storage Queues

## Prerequisites
- Laravel 5.2 - 8.x (not tested on previous versions)
- PHP 5.6+ for Laravel 5.2+
- PHP 7+ for Laravel 5.5+
- PHP 7.1+ for Laravel 5.6+
- PHP 7.2+ for Laravel 6+
- PHP 7.3+ for Laravel 8+
- Microsoft Azure Storage Account and Storage Account Key
- Queue container created through Azure Portal or via
[Azure CLI](https://docs.microsoft.com/en-us/cli/azure/storage/queue?view=azure-cli-latest#az-storage-queue-create)
or [PowerShell](https://docs.microsoft.com/en-us/azure/storage/queues/storage-powershell-how-to-use-queues#create-a-queue)

## Installation

### Install using composer
You can find this library on [Packagist](https://packagist.org/packages/squigg/azure-queue-laravel).

#### Important notes for Laravel 8
Laravel 8 has moved to Guzzle 7.x, but the upstream dependency `microsoft/azure-storage-queue` from this package still uses Guzzle 6.
This will cause `composer` to fail during dependency resolution.

Tests so far have not identified any impacting breaking changes between Guzzle 6 and 7, so while we wait for the upstream package to be updated, you can work around this issue by adding/updating your root
`composer.json` file to use an inline alias for `guzzlehttp/guzzle`:
```
"guzzlehttp/guzzle": "7.0.1 as 6.5.5"
```
Or run this command:
```
composer require guzzlehttp/guzzle:"7.0.1 as 6.5.5" 
```
#### Installation
Require this package in your `composer.json`. The version numbers will follow Laravel.
```
composer require abadmoab/azure-queue-laravel
```
    
Update Composer dependencies

```sh
composer update
```

## Configuration
#### Add Provider
If you are not using Laravel auto package discovery, add the ServiceProvider to your `providers` array in `config/app.php`:

	'Squigg\AzureQueueLaravel\AzureQueueServiceProvider',

For Lumen (5.x) you will need to add the provider to `bootstrap/app.php`:

        $app->register(Squigg\AzureQueueLaravel\AzureQueueServiceProvider::class);

#### Add Azure queue configuration
Add the following to the `connections` array in `config/queue.php`, and
fill out your own connection data from the Azure Management portal:

	'azure' => [
        'driver'        => 'azure',                             // Leave this as-is
        'protocol'      => 'https',                             // https or http
        'accountname'   => env('AZURE_QUEUE_STORAGE_NAME'),     // Azure storage account name
        'key'           => env('AZURE_QUEUE_KEY'),              // Access key for storage account
        'queue'         => env('AZURE_QUEUE_NAME'),             // Queue container name
        'timeout'       => 60,                                  // Seconds before a job is released back to the queue
        'endpoint'      => env('AZURE_QUEUE_ENDPOINTSUFFIX'),   // Optional endpoint suffix if different from core.windows.net
    ],

Add environment variables into your `.env` file to set the above configuration parameters:
    
    AZURE_QUEUE_STORAGE_NAME=xxx
    AZURE_QUEUE_KEY=xxx
    AZURE_QUEUE_NAME=xxx
    AZURE_QUEUE_ENDPOINTSUFFIX=xxx
    
#### Set the default Laravel queue
Update the default queue used by Laravel by setting the `QUEUE_CONNECTION` value in your `.env` file to `azure`.

    QUEUE_CONNECTION=azure

This setting is `QUEUE_DRIVER` in older versions of Laravel.

## Usage
Use the normal Laravel Queue functionality as per the [documentation](http://laravel.com/docs/queues).

Remember to update the default queue by setting the `QUEUE_DRIVER` value in your `.env` file to `azure`.

## Changelog

2020-09-19 - V8.0 - Support for Laravel 8.x (composer dependency and test refactoring only)

2020-06-04 - V7.0 - Support for Laravel 7.x (composer dependency and test refactoring only)

2020-06-04 - V6.0 - Support for Laravel 6.x (composer dependency changes only)

2019-07-13 - V5.8 - Support for Laravel 5.8 (composer dependency and test changes only)

2019-07-13 - V5.7.1 - Fix invalid signature on call to base Laravel Queue method

2018-09-04 - V5.7 - Support for Laravel 5.7 (composer dependency changes only)

2018-02-07 - V5.6 - Switch to GA version of Microsoft Azure Storage PHP API. Support Laravel 5.6 (composer.json changes
only). Update dev dependencies to latest versions.

2017-09-11 - V5.5 - Support Laravel 5.5 and PHP7+ only. Update Azure Storage API to 0.18

2017-09-11 - V5.4 - Update Azure Storage API to 0.15 (no breaking changes)

## License
Released under the MIT License. Based on [Alex Bouma's Laravel 4 package](https://github.com/stayallive/laravel-azure-blob-queue), updated for Laravel 5.
