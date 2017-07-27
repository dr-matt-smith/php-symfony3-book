

# Solving problems with Symfony \label{appendix_problem_solving}


## No home page loading

Ensure web server is running (either from console, or a webserver application with web root of the project's `/web` directory).

Point your web browser to the `app_dev.php` front controller script, e.g.:

```
    http://localhost:8000/app_dev.php
```

## "Route not Found" error after adding new controller method

If you have issues of Symfony not finding a new route you've added via a controller annotation comment, try this:

- delete directory `/var/cache`

Symfony caches (stores) routing data and also rendered pages from Twig, to speed up reponse time. But if you have changed controllers and routes, sometimes you have to manually delete the cache to ensure all new routes are checked against new requests.

## Issues with timezone

Try adding the following construction to `/app/AppKernel.php` to solve timeszone problems:

```php
    public function __construct($environment, $debug)
    {
        date_default_timezone_set( 'Europe/Dublin' );
        parent::__construct($environment, $debug);
    }
```

## Issues with Symfony 3 and PHPUnit.phar

Symfon 3.2 has issues with PHPUnit (it's PHPUnit's fault!). You can solve the problem with the Symphony PHPUnit `bridge` - which you install via Composer:

```bash
    composer require --dev symfony/phpunit-bridge
```

You then execute your PHPUnit test with the `simple-phpunit` command in `/vendor/bin` as follows:

```bash
    ./vendor/bin/simple-phpunit
```

Source:

- [Symfony Blog December 2016](http://symfony.com/blog/how-to-solve-phpunit-issues-in-symfony-3-2-applications)

## PHPUnit installed via Composer

To install PHPUnit with Composer run the following Composer update CLI command:

```bash
    composer require --dev phpunit/phpunit ^6.1
```

To run tests in directory `/tests` exectute the following CLI command:

```bash
    ./vendor/bin/phpunit tests
```

Source:

- [Stack overflow](https://stackoverflow.com/questions/13764309/how-to-use-phpunit-installed-from-composer)

As always you can add a shortcut script to your `composer.json` file to save typing, e.g.:

```json
    "scripts": {
        "run":"php bin/console server:run",
        "test":"./vendor/bin/phpunit tests",

        ...
    }
```

