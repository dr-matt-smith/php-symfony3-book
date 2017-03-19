
# Introduction to testing in Symfony

## Doctrine Fixtures

- [Doctrine Fixtures](http://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html)



## Using PHPUnit latest stable release

It's always recommend that you use the most recent **stable** release of any software tool or package. Therefore when testing with PHPUnit you should ensure you have the latest stable version. Check at the PHPUnit home page (see Figure \ref{phpunit_website}:

```
    https://phpunit.de/
```

![Current stable release on PHPUnit website. \label{phpunit_website}](./03_figures/testing/01_phpunit_website_sm.png)

The current stable release can be downloaded as a PHAR (PHp-ARchive) - it's usually the green one on the left...

## Issue (and solution) with polyfills (namespacing change)

The **PROBLEM**:

Due to an issue of the difference between `PHPUnit_Framework_TestCase` (pre-namespacing class naming convention) and `PHPUnit\Framework\TestCase` (namespacing convention), and a core PHP `bridge` tool, currently PHPUnit 6.x does not work with Symfony 3 without some tinkering with a kernel test case. You can read more about this here:

```
    https://github.com/symfony/symfony/issues/21534
```

So if we try to run PHPUnit we get this error message:

```
    PHP Fatal error:  Class 'PHPUnit_Framework_TestCase' not found in
    /home/user/project/vendor/symfony/symfony/src/Symfony/Bundle/FrameworkBundle/Test/KernelTestCase.php
    on line 23

```

So for now download and use PHPunit 5.7 (the orange, old stable release)

This issue will probably be solved in the next month or two, since both Symfony and PHPUnit are open source community projects and so people work hard and fast to solve compatibility issues with new releases.

The **SOLUTION**:

We need to change one line, in file `KernelTestCase.php`:

```php
    use PHPUnit\Framework\TestCase;
```

to this line:


```php
    use PHPUnit_Framework_TestCase as TestCase;
```

The location of `KernelTestCase.php` is:

```
    /vendor
        /symfony
            /symfony
                /src
                    /Symfony
                        /Bundle
                            /FrameworkBundle
                                /Test
```

Figure \ref{namespace_fix} shows the code having been corrected for `project20`.

![Fixed namespace so Symfony 3 works with PHPUnit 6. \label{namespace_fix}](./03_figures/testing/02_namespace_fix_sm.png)


## Testing in Symfony

Symfony is built by a test-test open source community. There is a lot of information about how to test Symfony in the offical documdentation pages:

- [Symfony testing](http://symfony.com/doc/current/testing.html)

- [Testing with user authentication tokens](http://symfony.com/doc/current/testing/simulating_authentication.html)

- [How to Simulate HTTP Authentication in a Functional Test](http://symfony.com/doc/current/testing/http_authentication.html)



