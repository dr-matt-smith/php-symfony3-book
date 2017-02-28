\mainmatter

# Introduction

## What is Symfony 3?

It's a PHP 'framework' that does loads for you, if you're writing a secure, database-drive web application.

## How to I need on my computer to get started?

I recommend you install the following:

- PHP 7 (on windows [Laragon](https://laragon.org/) works pretty well)
- a MySQL database server (on windows [Laragon](https://laragon.org/) works pretty well)
- a good text editor (I like [PHPStorm](https://www.jetbrains.com/phpstorm/specials/phpstorm/phpstorm.html?&gclid=CJTK_8SDrtICFWq-7Qodh98NpQ&gclsrc=aw.ds.ds&dclid=CNPY28WDrtICFQGn7QodqekBWg), but then it's free for educational users...)
- [Composer](https://getcomposer.org/) (PHP package manager - on windows [Laragon](https://laragon.org/) works pretty well)

or ... you could use something like [Cloud9](https://c9.io/dr_matt_smith), web-based IDE. You can get started on the free version and work from there ...

## How to I get started?

Either:

- install the Symfony command line installed, then create a project like this (to create a new project in a directory named `project01`):

```bash
        $ symfony new project01
```

or

- use Composer to create a new blank project for you, like this (to create a new project in a directory named `project01`):

```bash
        $  composer create-project symfony/framework-standard-edition project01
```

Learn about both these methods at the [Symfony download-installer page](http://symfony.com/download) and the [Symfony setup page](https://symfony.com/doc/current/setup.html)

or

- download one of the projects accompanying this book

## Where are the projects accompanying this book?

There are on Github:

- [https://github.com/dr-matt-smith/php-symfony3-book-codes](https://github.com/dr-matt-smith/php-symfony3-book-codes)

Download a project (e.g. `git clone URL`), then type `composer update` to download 3rd-party packages into a `/vendor` folder.


## How to I run a Symfony webapp?

### From the CLI
If you're not using a database engine like MySQL, then you can use the Symfony console command to 'serve up' your Symfony project from the command line

At the CLI (comamnd line terminal) ensure you are at the base level of your project (i.e. the same directory that has your `composer.json` file), and type the following:

```bash
    $ php bin/console server:run
```

### Webserver
If you are running a webserver (or combined web and database server like XAMPP or Laragon), then point your web server root to the `/web` folder - this is where public files go in Symfony projects.
