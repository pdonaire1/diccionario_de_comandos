# The next is a good tutorial about i18n translation in Django:

http://www.marinamele.com/taskbuster-django-tutorial/internationalization-localization-languages-time-zones

# Steps
**Configuration:**

- Add a folder called ``` locale ``` in our root dir

- Add the nexts things in our ``` settings.py ```:

Add:

```
MIDDLEWARE_CLASSES = (
   'django.contrib.sessions.middleware.SessionMiddleware',
   'django.middleware.locale.LocaleMiddleware',
   'django.middleware.common.CommonMiddleware',
)
.
.
.
USE_I18N = True
...
TEMPLATES = [
    {
        ...
        'OPTIONS': {
            'context_processors': [
                ...
                'django.template.context_processors.i18n',
            ],
        },
    },
]
....
from django.utils.translation import ugettext_lazy as _
LANGUAGES = (
    ('en', _('English')),
    ('ca', _('Catalan')),
)
....
LANGUAGE_CODE = 'en-us' # this is our preferencial language
LOCALE_PATHS = (
    os.path.join(BASE_DIR, 'locale'),
)
```

- In our urls.py:

```
...
from django.conf.urls.i18n import i18n_patterns
...
urlpatterns = [ # URLS without translation
    url(r'^page-without-translation$', page_without_translation, name='page-without-translation'),
    ...
]
urlpatterns += i18n_patterns( # We add ours URLS with translation
    url(r'^$', home, name='home'),
    url(r'^admin/', include(admin.site.urls)),
    ...
)

```

- Generate our locales with ``` python manage.py makemessages -l es ``` and ``` python manage.py makemessages -l en ```

After being created our ``` django.po ```, we need to compile the file with:

``` python mage.py compilemessages -l es ``` and ``` python mage.py compilemessages -l en ```

After all in our Python code:

```python
from django.utils.translation import ugettext as _
print _("Hello World")
>> "Hola Mundo"
```

- (Optional) test:

``` functional_tests/test_all_users.py ``` and do not forget compile our locales with 
``` python manage.py compilemessages -l es ```.

```python
def test_internationalization(self):
    for lang, h1_text in [('en', 'Welcome to MyApp!'),
                                ('es', 'Bienvenido a MyApp!')]:
        activate(lang)
        self.browser.get(self.get_full_url("home"))
        h1 = self.browser.find_element_by_tag_name("h1")
        self.assertEqual(h1.text, h1_text)
```
