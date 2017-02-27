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

    /**
     * Get id
     *
     * @return integer
     */
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
     php bin/console doctrine:generate:entities AppBundle
```

## Creating tables in the database


```bash
     php bin/console doctrine:schema:update --force
```
