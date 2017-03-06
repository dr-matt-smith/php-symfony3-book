
# Induction to Symfony sessions

## Getting a reference to the current session in Symfony

```php
    $session = new Session();
```

Note, you also need to add the following `use` statement for the class using this code:

```php
    use Symfony\Component\HttpFoundation\Session\Session;
```

Note - do **not** use any of the stardard PHP command for working with sessions. Do all you Symfony work through the Symfony session API. So, for example, do not use either of these PHP functions:

```php
    session_start();
    session_destroy();
```


You can now set/get values in the session by making reference to `$session`.

## Starting a session in Symfony (not really needed)

While a session will be started automatically if a session action takes places (if no session was already started), the Symfony documentation recommends your code starts a session if one is required. Here is the code to do so:

```php
    $session->start();
```

However, sometimes integrating this into your controller logic can be tricky (especially with controller redirects), so you may just trust Symofony to start a session when needed, and just use `$session = new Session()` to get a reference to the current (or newly created) session.

You'll get errors if you try to start an already started session ...

## Symfony's 2 session 'bags'  (`project11`)

We've already met sessions - the Symfony 'flash bag', which stores messages in the session for one request cycle.

Symfony also offers a second kind of session storage, session 'attribute bags', which store values for longer, and offer a namespacing approach to accessing values in session arrays.


Learn more at:

- [Symfony and sessions](http://symfony.com/doc/current/components/http_foundation/sessions.html)

## Shopping cart session attribute bag example

When you're leaning sessions, you need to build a 'shopping cart'! Let's imagine our students can select several subject elective 'modules', and store them in a 'basket' of electives.

We've created an `Elective` entity, and its CRUD controller and templates. So now let's add the 'shopping basket' functionality to add elective modules into a session basket.
