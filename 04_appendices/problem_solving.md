

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
