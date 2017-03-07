

# Solving problems with Symfony


## No home page loading

If you don't get the default Symfony home page, try this:

- copy the contents of `/web/app_dev.php` into `/web/app.php`

WARNING - this is just for now (we'll learn property Symfony configuration later). But this should get you going for now. You should NEVER do this for a project that might actually end up as a public production site!


## "Route not Found" error after adding new controller methor

If you have issues of Symfony not finding a new route you've added via a controller annotation comment, try this:

- delete directory `/var/cache`

Symfony caches (stores) routing data and also rendered pages from Twig, to speed up reponse time. But if you have changed controllers and routes, sometimes you have to manually delete the cache to ensure all new routes are checked against new requests.

## Issues with timezone \label{appendix_problem_solving}

Try adding the following construction to `/app/AppKernel.php` to solve timeszone problems:

```php
    public function __construct($environment, $debug)
    {
        date_default_timezone_set( 'Europe/Dublin' );
        parent::__construct($environment, $debug);
    }
```
