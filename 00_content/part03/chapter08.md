\mainmatter

# DIY forms

## Adding a form for new Student creation (`project05`)

Let's create a DIY (Do-It-Yourself) HTMl form to create a new student. We'll need:

- a controller method (and template) to display our new student form

    - route `/students/new`

- a controller method to process the submitted form data

    - route `/students/processNewForm`

The form will look as show in Figure \ref{new_student_form}.

![Form for a new student \label{new_student_form}](./03_figures/forms/1_new_student_form_sm.png)

## Twig new student form

Here is our new student form `/app/views/students/new.html.twig':

```html
    {% extends '_base.html.twig' %}
    {% block pageTitle %}new student form{% endblock %}

    {% block body %}
        <h1>Create new student</h1>

        <form action="/students/processNewForm" method="POST">
            Name:
            <input type="text" name="name">
            <p>
            <input type="submit" value="Create new student">
        </form>
    {% endblock %}
```

## Controller method (and annotation) to display new student form

Here is our `StudentController` method to display our Twig form:

```php
    /**
     * @Route("/students/new", name="students_new_form")
     */
    public function newFormAction(Request $request)
    {
        $argsArray = [
        ];

        $templateName = 'students/new';
        return $this->render($templateName . '.html.twig', $argsArray);
    }
```

We'll also add a link to this form route in our list of students page. So we add to the end of `/app/Resources/views/students/list.html.twig` the following link:

```html
        (... existing Twig code to show list of students here ...)

        <hr>
        <a href="{{ path('students_new_form')}}">
            create NEW student
        </a>
    {% endblock %}
```

## Controller method to process POST form data

We can access POST submitted data using the following expression:

```php
    $request->request->get(<POST_VAR_NAME>)
```

So we can extract and store in `$name` the POST `name` parameter by writing the following:

```php
    $name = $request->request->get('name');
```

Our full listing for `StudentController` method `processNewForm()` looks as follows:
```php
        /**
         * @Route("/students/processNewForm", name="students_process_new_form")
         */
        public function processNewFormAction(Request $request)
        {
            // extract 'name' parameter from POST data
            $name = $request->request->get('name');

            // forward this to the createAction() method
            return $this->createAction($name);
        }
```

Note that we then invokve our existing `createAction()` method, passing on the extracted `$name` string.

## Validating form data, and displaying temporary 'flash' messages in Twig (`project06`)

What should we do if an empty name string was submitted? We need to **validate** form data, and inform the user if there was a problem with their data.

Symfony offers a very useful feature called the 'flash bag'. Flash data exists for just 1 request and is then deleted from the session. So we can create an error message to be display (if present) by Twig, and we know some future request to display the form will no have that error message in the session any more.


## Three kinds of flash message: notice, warning and error

Typically we create 3 differnt kinds of flash notice:

- notice
- warning
- error

Our Twig template would style these differntly (e.g. pink background for errors etc.). Here is how to creater a flash message and have it stored (for 1 request) in the session:

```php
    $this->addFlash(
            'error',
            'Your changes were saved!'
        );
```

In Twig we can attempt to retrieve flash messages in the following way:

```html
    {% for flash_message in app.session.flashBag.get('notice') %}
        <div class="flash-notice">
            {{ flash_message }}
        </div>
    {% endfor %}
```
## Adding validation in our `processNewFormAction()z method

So let's add some validation logic to our processing of the new student form data:


## Adding flash display (with CSS) to our Twig template

First let's create a CSS stylesheet and ensure it is always loaded by adding its import into our `_base.html.twig` template.

First create the directory `css` in `/web` - remember that `/web` is the Symfony public folder, where all public images, CSS, javascript and basic front controllers (`app.php` and `app_dev.php`) are served from).

Now create CSS file `/web/css/flash.css` containing the following:

```css
    .flash-error {
        padding: 1rem;
        margin: 1rem;
        background-color: pink;
    }
```

Next we need to edit our `/app/Resources/views/_base.html.twig` so that every page in our webapp will have imported this CSS stylesheet. Edit the `<head>` element in `_base.html.twig` as follows:

```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8" />
            <title>MGW - {% block pageTitle %}{% endblock %}</title>

            <style>
                @import '/css/flash.css';
            </style>
            {% block stylesheets %}{% endblock %}
        </head>
```

## Adding validation logic to our form processing controller method

Now we can add the empty string test (and flash error message) to our `processNewFormAction()` method:

```php
    public function processNewFormAction(Request $request)
    {
        // extract 'name' parameter from POST data
        $name = $request->request->get('name');

        if(empty($name)){
            $this->addFlash(
                'error',
                'student name cannot be an empty string'
            );

            // forward this to the createAction() method
            return $this->newFormAction($request);
        }

        // forward this to the createAction() method
        return $this->createAction($name);
    }
```

So if the `$name` we extracted from the POST data is an empty string, then we add an `error` flash message into the session 'flash bag', and forward on processing of the request to our method to display the new student form again.

Finally, we need to add code in our new student form Twig template to display any error flash messages it finds. So we edit `/app/Resources/views/students/new.html.twig` as follows:

```html
    {% extends '_base.html.twig' %}
    {% block pageTitle %}new student form{% endblock %}

    {% block body %}

        <h1>Create new student</h1>

        {% for flash_message in app.session.flashBag.get('error') %}
            <div class="flash-error">
                {{ flash_message }}
            </div>
        {% endfor %}

        (... show HTML form as before ...)
```