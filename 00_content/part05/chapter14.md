
# Working with a session 'basket' of electives

## Debugging sessions in Twig

As well as the Symfony profiler, there is also the powerful Twig functiond `dump()`.
 This can be used to interrogate values in the session.
Figure \ref{twig_session_dump} shows our `electives[]` array in the session `Attribute Bag`, navigating through the Twig `dump()` output as follows:

```
    app> requestStack> requests[0]> session> storage> bags> attributes> electives[]
```




![Twig dump of session attribute bag. \label{twig_session_dump}](./03_figures/sessions/1_session_bags_in_app.png)

