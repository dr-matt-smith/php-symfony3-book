\mainmatter

# First steps

## It isn't working

If you don't get the default Sfymfony home page, try this:

- copy the contents of `/web/app_dev.php` into `/web/app.php`

WARNING - this is just for now (we'll learn property Symfony configuration later). But this should get you going for now. You should NEVER do this for a project that might actually end up as a public production site!


## All I get is the symfony home page (`project01`)

Figure \ref{page0}  is your basic, default Symfony home page if everything is up and running for a new Symfony project.

![New Symfony project home page. \label{page0}](./03_figures/introduction/0_default_homepage_sm.png)

## What we'll make (`project02`)

See Figure \ref{page1} for a screenshot of the new homepage we'll create this chapter.

![New home page. \label{page1}](./03_figures/introduction/1_home_page_sm.png)

There are 3 things Symfony needs to serve up a page (with the Twig templating system):

1. a route
2. a controller class and method
3. a Twig template

The first 2 can be combined, through the use of 'Anotation' comments, which declare the route in a comment immediately before the controller method defining the 'action' for that route, e.g.:

        /**
         * @Route("/", name="homepage")
         */
        public function indexAction(Request $request)
        {
            $argsArray =  [
                'name' => 'matt'
            ];

            $templateName = 'index';
            return $this->render($templateName . '.html.twig', $argsArray);
        }
    }

The last (Twig template) can be a single file, and a simpler template that 'extends' a base template (which has all the standard doctype, css, js and core HTML structure in it).

If don't know much about Twig then go off an learn it (you can learn it stand alone, with a simple micro-framework like Silex, and as part of learning Symfony).

Here is our `_base.html.twig` template:

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8" />
            <title>MGW - {% block pageTitle %}{% endblock %}</title>
            {% block stylesheets %}{% endblock %}
        </head>
        <body>

        <hr>
            {% block body %}
            {% endblock %}

            {% block javascripts %}{% endblock %}
        </body>
    </html>

Here is the template for our index page, `index.html.twig`:

    {% extends '_base.html.twig' %}

    {% block pageTitle %}home page{% endblock %}

    {% block body %}

        <h1>welcome to home page</h1>
        <ul>
            <li>
                <a href="{{ path('homepage') }}">back to home page</a>
            </li>
            <li>
                <a hrer="http://symfony.com/doc/current/page_creation.html">getting started (on the Symfony website)</a>
            </li>
        </ul>

        <p>
            I am the home page ...
        <br>
            my name is {{ name }}
        </p>

        {{ dump() }}
    {% endblock %}


## First - get rid of all that default page stuff (`project02`)

A new Symfony project places its `DefaultController` at this location:

    /src/AppBundle/Controller/DefaultController.php

Let's clear out the content of the controller, so there is no code in the body of the `indexAction()` method:

    class DefaultController extends Controller
    {
        /**
         * @Route("/", name="homepage")
         */
        public function indexAction(Request $request)
        {
        }
    }

(NOTE - leave all the 'uses' statements and the namespace, since they mean any classes we refer to, or annontations we use, all work correctly).

We want to use the template `index.html.twig`, since they all end in `.html.twig` let's concatenate that on later

        $templateName = 'index';

Twig templates expect to be given an associative array of any special data for the template, so let's illustrate this by passing a parameter `name` with your name (I'm Matt, so that will be my name parameter's value!):

    $argsArray =  [
        'name' => 'matt'
    ];

There is nothing magic about the array identifier `$argsArray` - it's just a habit I've go in to when teaching Twig to my students - so change this (and anything - it's **your** project) to become more confident with working with the different bits of Symfony.

Symfomy's `Controller` class offers a hand method `render()` with accesses the Twig service in the Symfony application, so we can just invoke this method passing the template name (and appending the `.html.twig` string), and the array of arguments:

    /**
     * @Route("/", name="homepage")
     */
    public function indexAction(Request $request)
    {
        $argsArray =  [
            'name' => 'matt'
        ];

        $templateName = 'index';
        return $this->render($templateName . '.html.twig', $argsArray);
    }

Note that this final statement is a `return` statement. Basically any web application received (and interprets the contents of) an HTTP 'request', and builds and sends back an HTTP 'response'. They way Symfony (and most MCV webapps) work is that the controller method invoked for a given route has the responsibility of building and returning a 'response' (or sometimes just the text 'content' of a response, and the MVC application will build an HTTP response around that text content.


