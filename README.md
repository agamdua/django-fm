This is experimental!

Django FM
=========

Requires jQuery and Bootstrap 3.

Install:

```bash
pip install django-fm
```

Add `crispy_forms` and 'fm' to INSTALLED_APPS:

```python
INSTALLED_APPS = (
    ...
    'crispy_forms',
    'fm',
)

Also in `settings.py` set crispy template pack to `bootstrap3`:

```python
CRISPY_TEMPLATE_PACK = 'bootstrap3'
```

To be continued...