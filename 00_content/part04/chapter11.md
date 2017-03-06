
# Generating entities from the CLI

## Generating an 'elective' module entity from the CLI

Continuing our student/college example project, let's consider the case where  students can select several subject elective 'modules', and store them in a 'basket' of electives. We'll learn about sessions for the shopping basket functionality in the next part, so for now let's create the `Elective` entity and use some CRUD to enter some records in the database.

We are going to use Doctrine's interactive CLI command to create class `AppBundle/Entity.php` for us. Entities have an integer `id` AUTO-INCREMENT primary key by default, so we just need to ask Doctrine to add string fields for `moduleCode` and `moduleTitle`, and an integer number of academic `credits` field
- ensure your Webserver is running before working with Doctrine ...

php bin/console generate:doctrine:entity --entity=AppBundle:Elective

First let's tell Doctrine that we want to create a new entity `Elective` in our `AppBundle`:

```bash
    $ php bin/console generate:doctrine:entity --entity=AppBundle:Elective
```

Doctrine then tells us what is doing:

```bash
    Welcome to the Doctrine2 entity generator

    This command helps you generate Doctrine2 entities.
```

Then Doctrine tells us we need an entity 'shortcut name', but it also offers us one in square brackets, which we can accept by pressing `<Return>`:

```bash
    First, you need to give the entity name you want to generate.
    You must use the shortcut notation like AcmeBlogBundle:Post.

    The Entity shortcut name [AppBundle:Elective]:
```

Next Doctrine asks us how we will declare the mapping information between this entity and the database table, again it offers us a default (annotation) in square brackets, we we accept by  pressing `<Return>`:

```bash
    Determine the format to use for the mapping information.

    Configuration format (yml, xml, php, or annotation) [annotation]:
```

Finally Doctrine asks us to start describing each field we want.

```bash
    Instead of starting with a blank entity, you can add some fields now.
    Note that the primary key will be added automatically (named id).

    Available types: array, simple_array, json_array, object,
    boolean, integer, smallint, bigint, string, text, datetime, datetimetz,
    date, time, decimal, float, binary, blob, guid.
```

Each field needs:

- field name
- field type
- field length (if string, not needed for some fields, like integer)
- Is nullable
- Unique

In most cases all we need to do is name the field, and either accept the default `string` data type (or correct it to integer or decimal), and then accept the defailts for the remaining field properties.

So let's create a string field `moduleCode`. Since `string` is the default, all we need to type is the field name and then press `<Return>` to accept the remaining defaults:

```bash
    New field name (press <return> to stop adding fields): moduleCode
    Field type [string]:
    Field length [255]:
    Is nullable [false]:
    Unique [false]:
```

Let's do the same for string field `moduleCode`:

```bash
    New field name (press <return> to stop adding fields): moduleTitle
    Field type [string]:
    Field length [255]:
    Is nullable [false]:
    Unique [false]:
```

Now we'll declare `integer` field `credits`. Don't worry, you don't have to type out the whole word `integer` - the CLI command will spot what you're typing after a couple of characters and you can accept it by pressing, you've guessed it, `<Returnu>`:

``` bash
    New field name (press <return> to stop adding fields): credits
    Field type [string]: integer
    Is nullable [false]:
    Unique [false]:
```

When we've declared all the fields we wish to at this time, we just press `<Return>` when asked for the next field;s name:

```bash
    New field name (press <return> to stop adding fields):
```


Doctrine then goes off to create our Entity class, with all its getters and setters. and prints our a confirmation message of success, and telling us it created both an Entity class `Entity/Elective.php` and an associated Repository class `Repository/ElectiveRepository.php` for Bundle `AppBundle`:

```bash
    Entity generation

      created ./src/AppBundle/Entity/Elective.php
    > Generating entity class src/AppBundle/Entity/Elective.php: OK!
    > Generating repository class src/AppBundle/Repository/ElectiveRepository.php: OK!

    Everything is OK! Now get to work :).
```

See Appendix \ref{appendix_entity_gen} for another example of interactive CLI entity generation with the Doctrine command line tool.


## Creating tables in the database

Now our entity  `Elective` is completed, we can tell Doctrine to create a corresponding table in the database (or ALTER the table in the database if one previously existed):

```bash
     $ php bin/console doctrine:schema:update --force
```
