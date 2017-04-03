

# Fixtures - setting up a database state

## Initial values for your project database

Fixtures play two roles:

- inserting initial values into your database (e.g. the first `admin` user)
- setting up the database to a known state for **testing** purposes

Doctrine provides a Symfony fixtures **bundle** that makes things very straightforward.

Learn more about Symfony fixtures at:

- [Symfony website fixtures page](https://symfony.com/doc/master/bundles/DoctrineFixturesBundle/index.html)

## Installing and registering the fixtures bundle

### Install the bundle
Use Composer to install the bunder in the the `/vendor` directory:

```bash
    composer require --dev doctrine/doctrine-fixtures-bundle
```

### Resister bundle with the Symfony application
We need to 'register' this new bundle in the `AppKernel` class. This can be found in directory `/app`.
There are already several bundles registered, just add the Doctrine Fixtures bundle to the end of the `dev/test` part of the `registerBundles()` method (since we don't need this bundle for the production running of the web application):

```php
    // file: /app/AppKernel.php
    class AppKernel extends Kernel
    {
        public function registerBundles()
        {
            $bundles = [
                new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
                ...
            ];

            if (in_array($this->getEnvironment(), ['dev', 'test'], true)) {
                $bundles[] = new Symfony\Bundle\DebugBundle\DebugBundle();
                ...
                $bundles[] = new Doctrine\Bundle\FixturesBundle\DoctrineFixturesBundle();
            }
```

## Writing the fixture classes

We need to locate our fixtures in our `/src/AppBundle` directory, inside a `DataFixtures` directory, and in that a directory named for whether we are using ORM or ODM. Since we're using the Doctrine ORM then the path for our data fixtures classes should be `/src/AppBundle/DataFixtures/ORM/`.

Fixture classes need to implement the interfaces, `FixtureInterface`. In example below our code also needs to access the container, and so it also implemenets the `ContainerAwareInterface`.

Here is a class to create 2 users for entity `AppBundle\Entity\User`:

```php
    // src/AppBundle/DataFixtures/ORM/LoadUserData.php

    namespace AppBundle\DataFixtures\ORM;

    use Doctrine\Common\DataFixtures\FixtureInterface;
    use Doctrine\Common\Persistence\ObjectManager;
    use AppBundle\Entity\User;
    use Symfony\Component\DependencyInjection\ContainerAwareInterface;
    use Symfony\Component\DependencyInjection\ContainerInterface;


    class LoadUserData implements FixtureInterface, ContainerAwareInterface
    {
```

We need a private property `$container` that will be set by method `setContainer(...)`:
```php
        /**
         * @var ContainerInterface
         */
        private $container;

        public function setContainer(ContainerInterface $container = null)
        {
            $this->container = $container;
        }
```

We need a `load(...)` method, that gets invoked when we are loading fixtures from the CLI. This method creates objects for the entities we want in our databse, and the saves (persists) them to the database:
```php
        public function load(ObjectManager $manager)
        {
            // create objects
            $userAdmin = $this->createActiveUser('admin', 'admin@admin.com', 'admin');
            $userMatt = $this->createActiveUser('matt', 'matt@matt.com', 'smith');

            // store to DB
            $manager->persist($userAdmin);
            $manager->persist($userMatt);
            $manager->flush();
        }
```

Rather than put all the work in the `load(...)` method, we can create a helper method to create each new object. Method `createActiveUser(...)` creates and returns a reference to a new `User` object given some parameters:
```php

        private function createActiveUser($username, $email, $plainPassword):User
        {
            $user = new User();
            $user->setUsername($username);
            $user->setEmail($email);
            $user->setIsActive(true);

            // password - and encoding
            $plainPassword = 'admin';
            $encodedPassword = $this->encodePassword($user, $plainPassword);
            $user->setPassword($encodedPassword);

            return $user;
        }
```

Finally, to make it clear how we are encoding the password, we have method `encodePassword(...)`, returning an encoded password give a `User` object and a plain password:
```php
        private function encodePassword($user, $plainPassword):string
        {
            $encoder = $this->container->get('security.password_encoder');
            $encodedPassword = $encoder->encodePassword($user, $plainPassword);
            return $encodedPassword;
        }
    }
```

## Loading the fixtures

Loading fixtures involves deleting all existing database contents and then creating the data from the fixture classes - so you'll get a warning when loading fixtures. At the CLI type:

```bash
    php bin/console doctrine:fixtures:load
```

Figure \ref{load_fixtures} shows the CLI output when you load fixtures.

![Using CLI to load database fixtures. \label{load_fixtures}](./03_figures/database/10_load_fixtures_sm.png)

Figure \ref{fxtures_in_db} shows the data inserted into the database by our fixtures class. Note, this screenshot is from **Adminer** a lightweight PHP-based DB GUI. See Appendix \ref{appendix_adminer} to learn how to set up this utility.

![Using CLI to load database fixtures. \label{fxtures_in_db}](./03_figures/database/11_data_in_db_sm.png)
