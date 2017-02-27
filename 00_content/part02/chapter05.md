\mainmatter

# Working with Entity classes

## A `Student` entity class

Doctrine expects to find entity classes in a directory named `Entity`, so let's create one and move our `Student` class there. We can also delete class `StudentRepository` since Doctrine will create repository classes automatically for our entities (which we can edit if we need to later to add project-specific methods).

Do the following:

1. create directory `/src/AppBundle/Entity`
1. move class `Student` to this new directory
1. delete class `StudentRepository`

We also need to add to the namespace inside class `Student`, changing it to `AppBundle\Entity`. We also need to remove all methods, since Doctrine with create getter and setters etc. automatically. So edit class `Student` to look as follows, i.e. just listing the properties 'id' and 'name':

```php
    namespace AppBundle\Entity;

    class Student
    {
        private $id;
        private $name;
    }
```

## Using annotation comments to declare DB mappings

We need to tell Doctrine what table name this entity should map to, and also confirm the data types of each field. We'll do this using annotation comments (although this can be also be declare in separate YAML or XML files if you prefer). We need to add a `use` statement and we define the namespace alias `ORM` to keep our comments simpler.

Our first comment is for the class, stating that it is an ORM entity and mapping it to database table `students`:

```php
    namespace AppBundle\Entity;

    use Doctrine\ORM\Mapping as ORM;

    /**
     * @ORM\Entity
     * @ORM\Table(name="students")
     */
    class Student
```

## Declaring types for fields

We now use annotations to declare the types (and if appropriate, lengths) of each field. Also for the 'id' we need to tell it to `AUTO_INCREMENT` this special field.

```php
    /**
     * @ORM\Column(type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=100)
     */
    private $name;
```

## Valdiate our annotations

We can now validate these values. This command performs 2 actions, it checks our annotation comments, it also checks whether these match with the structure of the table the database system. Of course, since we haven't yet told Doctring to create the actual database table, this second check will fail at this point in time.

```
    $ php bin/console doctrine:schema:validate
```

The output should be something like this (if our comments are valid):

```
    [Mapping]  OK - The mapping files are correct.
    [Database] FAIL - The database schema is not in sync with the current mapping file.
```

## Generating getters and setters

We can tell Doctrine to complete the creation of the entity class with the `generate:entities` command:

```bash
   php bin/console doctrine:generate:entities AppBundle/Entity/Student
```

We can also add our **own** logic to the entity class, for any special getters etc.

You can tell Doctrine to generate all entities for a given 'bundle' (but ?? it may overwrite any edits you've made to entites^[NOTE TO SELF - CHECK THIS WHEN YOU HAVE A CHANCE])

```bash
     $ php bin/console doctrine:generate:entities AppBundle
```

So we now have getters and setters (no setter for ID since we don't change the AUTO INCREMENTED db ID value) added to our class `Student`:

```php

        /**
         * Get id
         *
         * @return integer
         */
        public function getId()
        {
            return $this->id;
        }

        /**
         * Set name
         *
         * @param string $name
         *
         * @return Student
         */
        public function setName($name)
        {
            $this->name = $name;
            return $this;
        }

        /**
         * Get name
         *
         * @return string
         */
        public function getName()
        {
            return $this->name;
        }
```

## Creating tables in the database

Now our entity  `Student` is completed, we can tell Doctrine to create a corresponding table in the database (or ALTER the table in the database if one previously exisited):

```bash
     $ php bin/console doctrine:schema:update --force
```

if all goes well you'll see a couple of confirmation messages after entering the command above:

```bash
    Updating database schema...
    Database schema updated successfully! "1" query was executed
    $
```

You should now see a new table in the database in your DB manager. Figure \ref{new_table} shows our new `students` table created for us.

![CLI created table in PHPMyAdmin. \label{new_table}](./03_figures/database/2_new_table.png)


## Generating entities from an existing database

Doctrine allows you to generated entites matching tables in an existing database. Learn about that from the Symfony documentation pages:

- [Symfony docs on inferring entites from existing db tables](http://symfony.com/doc/current/doctrine/reverse_engineering.html)

3_new_student.png