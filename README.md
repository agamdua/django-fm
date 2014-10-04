[Live example](http://djangofm.herokuapp.com/) - see the source code of demonstration in `demo` repository folder.

Django FM
=========

Requires jQuery and Bootstrap 3 on client side. Depends on Django-crispy-forms.

This app allows to make responsive AJAX modal forms for creating, editing, deleting objects in Django. This is a very personalized approach to quickly build admin-like interfaces. It reduces an amount of time and code when making tedious Django work. 

Install:

```bash
pip install django-fm
```

Add `crispy_forms` and `fm` to INSTALLED_APPS:

```python
INSTALLED_APPS = (
    ...
    'crispy_forms',
    'fm',
)
```

Also in `settings.py` set crispy template pack to `bootstrap3`:

```python
CRISPY_TEMPLATE_PACK = 'bootstrap3'
```

Include modal template into your project template and initialize jQuery plugin:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css"/>
        <script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
        <script type="text/javascript" src="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
    </head>
    <body>
        {% block content %}{% endblock %}
        {% include "fm/modal.html" %}
        <script type="text/javascript">
            $(function() {
                $.fm({debug: true});
            });
        </script>
    </body>
</html>
```

There are 3 class-based views in django-fm to inherit from when you want AJAX forms:

* AjaxCreateView
* AjaxUpdateView
* AjaxDeleteView

You create urls for them as usual, in templates you just create links to create, update, delete resources with special class (`fm-create`, `fm-update`, `fm-delete`).

So let's create a view to create new instance of some model. In `views.py`:

```python
from fm.views import AjaxCreateView
from feedback.forms import FeedbackForm

class FeedbackCreateView(AjaxCreateView):
    form_class = FeedbackForm
```

That's all in simple case - you are just inherit from `AjaxCreateView` and provide `form_class` argument - you do this every day in Django, right?

Also you should create url for this resource in `urls.py`:

```python
from django.conf.urls import patterns, url
from feedback.views import FeedbackCreateView

urlpatterns = patterns(
    'feedback.views',
    ...
    url(r'^create/$', FeedbackCreateView.as_view(), name="feedback_create"),
    ...
)
```

Again - nothing new here.

The most interesting part in  template - you don't have to define template for your form - just write a ling to creating new object with special attributes which tell django-fm how to behave.

So in your template write:

```html
<a href="{% url 'feedback_create' %}" class="fm-create" data-fm-head="Create" data-fm-callback="reload">Create new</a>
```

Look at `fm-create` special class - it's necessary. And that's all - now when user click on this link - modal AJAX window with form will be shown.

Every link can have some attributes which define modal window behaviour and callback after successfull object creation, update or deletion:

* `data-fm-head` - header of modal
* `data-fm-action` - what to do after successfull modal submission - at moment the following values allowed: `reload`, `redirect`, `replace`, `remove`, `prepend`, `append`
* `data-fm-target` - value of action specific for each action type - for example this must be an URL when `data-fm-action` is `redirect`

See demo project to see this concept in action
