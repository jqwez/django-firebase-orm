
[![Build Status](https://travis-ci.com/TralahM/django-firebase-orm.svg?branch=master)](https://travis-ci.com/TralahM/django-firebase-orm)
[![Build status](https://ci.appveyor.com/api/projects/status/yvvmq5hyf7hj743a/branch/master?svg=true)](https://ci.appveyor.com/project/TralahM/django-firebase-orm/branch/master)
[![Documentation Status](https://readthedocs.org/projects/django-firebase-orm/badge/?version=latest)](https://django-firebase-orm.readthedocs.io/en/latest/?badge=latest)
[![License: GPLv3](https://img.shields.io/badge/License-GPLV2-green.svg)](https://opensource.org/licenses/GPLV2)
[![Organization](https://img.shields.io/badge/Org-TralahTek-blue.svg)](https://github.com/TralahTek)
[![Views](http://hits.dwyl.io/TralahM/django-firebase-orm.svg)](http://dwyl.io/TralahM/django-firebase-orm)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=flat-square)](https://github.com/TralahM/django-firebase-orm/pull/)
[![GitHub pull-requests](https://img.shields.io/badge/Issues-pr-red.svg?style=flat-square)](https://github.com/TralahM/django-firebase-orm/pull/)
[![Language](https://img.shields.io/badge/Language-python-3572A5.svg)](https://github.com/TralahM)
<img title="Watching" src="https://img.shields.io/github/watchers/TralahM/django-firebase-orm?label=Watchers&color=blue&style=flat-square">
<img title="Stars" src="https://img.shields.io/github/stars/TralahM/django-firebase-orm?color=red&style=flat-square">
<img title="Forks" src="https://img.shields.io/github/forks/TralahM/django-firebase-orm?color=green&style=flat-square">

# django-firebase-orm.

forked from [TralahM](https://github.com/TralahM/django-firebase-orm) to support building a Django app that leverage Firebase Realtime Database.

Django like models for NoSQL database Firestore Integrating with
django\'s ORM. This is a fork and improvement of
[firebase_orm](https://github.com/joewalk102/firebase-orm) project which
was initially forked from [xavx0z](https://github.com/xavx0z) and
stopped maintaining it.

I made a few changes and improvements to suit my liking:

1.  Changed the need of creating a second settings.py file in the root
    of your django project to now only require that you define the
    neccessary configurations in your project\'s settings module. *Note
    that django is now an explicit dependency*
2.  Created a new package for this app on pypi under
    **django-firebase-orm**

It is my desire to continue the development of this project and thus
welcome all developers wishing to contribute via improving
documentation, bug fixes, test coverage, new features, etc.

Installation
------------

```shell
$ pip install django-firebase-orm
```

Initialize
----------

In your project settings add the following configuration variables

> settings.py
>
```python
FIREBASE_ORM_CERTIFICATE = 'path/to/serviceAccountKey.json'
FIREBASE_ORM_BUCKET_NAME = '<BUCKET_NAME>.appspot.com'
```

FIREBASE_ORM_CERTIFICATE

:   Once you have created a [Firebase
    console](https://console.firebase.google.com/?authuser=0) project
    and downloaded a JSON file with your service account credentials.

FIREBASE_ORM_BUCKET_NAME

:   The bucket name must not contain gs:// or any other protocol
    prefixes. For example, if the bucket URL displayed in the [Firebase
    console](https://console.firebase.google.com/?authuser=0) is
    gs://bucket-name.appspot.com, pass the string
    bucket-name.appspot.com

Usage
-----

### Create model:

```python
from firebase_orm import models


class Article(models.Model):
    headline = models.TextField()
    type_article = models.TextField(db_column='type')

    class Meta:
        db_table = 'medications'

    def __str__(self):
        return self.headline
```

### Using The API:

**Creating objects**

To represent cloud firestore data in Python objects, FirebaseORM uses an
intuitive system: A *model* *class* represents a *collection*, and an
*instance* of that class represents a *document* in collection.

To create an object, instantiate it using keyword arguments to the model
class, then call save() to save it to the database.

```pycon
# Import the models we created
>>> from models import Article
# Create a new Article.
>>> a = Article(headline='Django is cool')
# Save the object into the database. You have to call save() explicitly.
>>> a.save()
```

**Retrieving all objects**

The simplest way to retrieve documents from a collections is to get all
of them. To do this, use the all() method on a Manager as you would in
normal django:

```pycon
>>> all_Article = Article.objects.all()
```

The all() method returns a list instance Article of all the collection
in the database.

```pycon
# Now it has an ID.
>>> a.id
1
# Fields are represented as attributes on the Python object.
>>> a.headline
'Django is cool'
```

**Saving changes to objects**

To save changes to an object that's already in the database, use save().

Given a Article instance a that has already been saved to the database,
this example changes its name and updates its record in the database:

```pycon
>>> a.headline = 'Django-Firebase-ORM is awesome'
>>> a.save()
```

This performs an document.update() method behind the scenes. FirebaseORM
doesn't hit the database until you explicitly call save().

```pycon
# Firebase ORM provides a rich database lookup API.
>>> Article.objects.get(id=1)
<Article: Django-Firebase-ORM is awesome>
>>> Article.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Article matching query does not exist.
```

Field options:
--------------

The following arguments are available to all field types. All are
optional.

**Field.db_column**

> If contains characters that aren't allowed in Python variable names --
> use db_column. The name of the firestore key in document to use for
> this field. If this isn't given, FirebaseORM will use the field's
> name.

Field types:
------------

### AutoField

**class AutoField()**

> By default, FirebaseORM gives each model the following field:
>

```python
id = models.AutoField(primary_key=True)
```

### TextField

**class TextField(**options)\*\*

> Text string Up to 1,048,487 bytes (1 MiB - 89 bytes). Only the first
> 1,500 bytes of the UTF-8 representation are considered by queries.
>
> TextField has not extra required argument.

Dependencies
------------

1.  *firebase-admin*
2.  *grpcio*
3.  *django*

## Building from Source for Developers

```console
$ git clone https://github.com/TralahM/django-firebase-orm.git
$ cd django-firebase-orm
```

# Contributing
[See the Contributing File](CONTRIBUTING.rst)


[See the Pull Request File](PULL_REQUEST_TEMPLATE.md)

# LICENCE

[Read the license here](LICENSE)

CREDITS
-------

Thanks to [joewalk102](https://github.com/joewalk102) for forking the
original project without whom this project would not be possible.

[![TralahTek](https://img.shields.io/badge/Organization-TralahTek-black.svg?style=for-the-badge&logo=github)](https://github.com/TralahTek)
[![TralahM](https://img.shields.io/badge/Engineer-TralahM-blue.svg?style=for-the-badge&logo=github)](https://github.com/TralahM)
[![TralahM](https://img.shields.io/badge/Maintainer-TralahM-green.svg?style=for-the-badge&logo=github)](https://github.com/TralahM)


# Self-Promotion

[![](https://img.shields.io/badge/Github-TralahM-green?style=for-the-badge&logo=github)](https://github.com/TralahM)
[![](https://img.shields.io/badge/Twitter-%40tralahtek-red?style=for-the-badge&logo=twitter)](https://twitter.com/TralahM)
[![TralahM](https://img.shields.io/badge/Kaggle-TralahM-purple.svg?style=for-the-badge&logo=kaggle)](https://kaggle.com/TralahM)
[![TralahM](https://img.shields.io/badge/LinkedIn-TralahM-red.svg?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/TralahM)


[![Blog](https://img.shields.io/badge/Blog-tralahm.tralahtek.com-blue.svg?style=for-the-badge&logo=rss)](https://tralahm.tralahtek.com)

[![TralahTek](https://img.shields.io/badge/Organization-TralahTek-cyan.svg?style=for-the-badge)](https://org.tralahtek.com)


