
# Doctrine the ORM

## What is an ORM?

The acronym ORM stands for:

- O: Object
- R: Relational
- M: Mapping

In a nutshell projects using an ORM mean we write code relating to collections of related **objects**, without having to worry about the way the data in those objects is actually represented and stored via a database or disk filing system or whatever. This is an example of 'abstraction' - adding a 'layer' between one software component and another. DBAL is the term used for separating the database interactions completed from other software components. DBAL stands for:

- DataBase
- Abstraction
- Layer

With ORMs we can interactive (CRUD^[CRUD = Create-Read-Update-Delete])
with persistent object collections either using methods of the object repositories (e.g. `findAll()`, `findOneById()`, `delete()` etc.), or using SQL-lite languages. For example Symfony uses the `Doctrine` ORM system, and that offers `DQL`, the Doctrine Query Language.

You can read more about ORMs and Symfony at:

- [Doctrine project's ORM page](http://www.doctrine-project.org/projects/orm.html)
- [Wikipedia's ORM page](https://en.wikipedia.org/wiki/Object-relational_mapping)
- (Symfony's Doctrine help pages)[http://symfony.com/doc/current/doctrine.html]

## Quick start
Once you've learnt how to work with Entity classes and Doctrine, these are the 3 commands you need to know (executed from the CLI console `php bin/console ...`):

1. `doctrine:database:create`
1. `doctrine:database:migrate` (or possibly `doctrine:schema:update --force`)
1. `doctrine:fixtures:load`

This should make sense by the time you've reached the end of this chapter.

## Setting up your project to work with MySQL or SQLite

You need to decide which database system you'll use for your project, and then configure the project with details of the database driver, host/path etc. See the following Appendices to learn about these issues, and to find specific instructions for both MySQL and SQLite (both are very easy to setup for Symfony):

- Appendix \ref{appendix_parameters} describing the parameter and config files in `/app/config`

- Appdenix \ref{appendix_db_mysql} describing how to set up a project for MySQL

- Appdendix \ref{appendix_db_sqlite} describing how to set up a project for SQLite

    NOTE this appendix also includes a link to the SQLite website page helping you decide whether SQLite is suitable


If you aer working on a small project / small website, often you'll find SQLite easier to setup and faster to work with (since you don' need to run any database server etc.). So it's worth a few minutes thinking before choosing which database system to work with before you go ahead and configure your project.



