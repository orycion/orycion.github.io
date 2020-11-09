---
layout: post
title: Debugging SQL queries in Django
author: Thomas Leese
---

Have you ever had a query not return the right results or perhaps take much longer than it should do? Or maybe you’ve just been curious to know what queries Django is running underneath?

Django comes with a built in way of logging all the SQL queries that it makes which can be incredibly useful. It’s pretty simple, just add this to your settings.py file:

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
        }
    },
    'loggers': {
        'django.db.backends': {
            'level': 'DEBUG',
            'handlers': ['console'],
        },
    },
}
```

When you run queries in the console, and from the web server, you’ll be able to see them in the console:

```python
>>> from gym.models import Workout

>>> Workout.objects.all()

(0.004) SELECT "gym_workout"."id", "gym_workout"."start_time", "gym_workout"."user_id" FROM "gym_workout" ORDER BY "gym_workout"."start_time" ASC  LIMIT 21; args=()
```

If you don’t want to log all queries, but you have access to a QuerySet object you can call .query and print the result to see the underlying query:

```python
>>> print(Workout.objects.all().query)

SELECT "gym_workout"."id", "gym_workout"."start_time", "gym_workout"."user_id" FROM "gym_workout" ORDER BY "gym_workout"."start_time" ASC
```

If you want even more information, there’s a great app you can add to your project called the [Django Debug Toolbar](https://django-debug-toolbar.readthedocs.io/en/latest/) which lets you see the queries happening while you navigate around your website.

![Django Debug Toolbar](/assets/posts/2019-02-28-debug-sql-queries-django/screenshot.png)

It’s pretty simple to set up:

```sh
$ pipenv install django-debug-toolbar
```

And then add it to your `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...,
    'django.contrib.staticfiles',
    ...,
    'debug_toolbar',
]
STATIC_URL = '/static/'
```

For more information you can see [the installation instructions for Django Debug Toolbar](https://django-debug-toolbar.readthedocs.io/en/latest/installation.html#getting-the-code).
