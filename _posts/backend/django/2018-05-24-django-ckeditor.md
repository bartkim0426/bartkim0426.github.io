---
layout: post
title:  "Django: use ckeditor in django - django ckeditor"
categories: django
---

### Install

Install with pip

```bash
pip install django-ckeditor
```

Add `ckedtor` to `INSTALLED_APPS`


And collectstatic command for add `ckeditor` static files.

```bash
python manage.py collectstatic
```

> In dev environment, just copy `ckeditor` directory from `staticfiles` after `collectstatic`


And add `CKEDITOR_BASEPATH` for `ckedtor` directory unber `staticfiles` (STATIC_ROOT)

```python
CKEDITOR_BASEPATH = "/static/ckeditor/ckeditor"
```

And add `window.CKEDITOR_BASEPATH` in `admin/change_form.html`.


{% raw %}
```html
{% extends "admin/change_form.html" %}

{% block extrahead %}
<script>window.CKEDITOR_BASEPATH = '/static/ckeditor/ckeditor/';</script>
{{ block.super }}
{% endblock %}
```
{% endraw %}


### Add ckeditor_uploader for file upload

Add `ckeditor_uploader` in `INSTALLED_APPS`


And add `CKEDITOR_UPLOAD_PATH`.

```python
CKEDITOR_UPLOAD_PATH = "uploads/"
```

And add ckeditor to urls

```python
url(r'^ckeditor/', include('ckeditor_uploader.urls')),
```



### Customizing ckeditor

There's many config in ckeditor.


Here's example ckeditor configuration. You can add plugins and add it to `extraPlugins`. (Download plugins in ckeditor webpage and add it to `ckeditor/plugins/`)

```python
CKEDITOR_CONFIGS = {
    'default': {
        'skin': 'moono',
        # 'skin': 'office2013',
        'toolbar_Basic': [
            ['Source', '-', 'Bold', 'Italic']
        ],
        'toolbar_YourCustomToolbarConfig': [
            {'name': 'document', 'items': ['Source', '-', 'Save', 'NewPage', 'Preview', 'Print', '-', 'Templates']},
            {'name': 'clipboard', 'items': ['Cut', 'Copy', 'Paste', 'PasteText', 'PasteFromWord', '-', 'Undo', 'Redo']},
            {'name': 'editing', 'items': ['Find', 'Replace', '-', 'SelectAll']},
            {'name': 'forms',
             'items': ['Form', 'Checkbox', 'Radio', 'TextField', 'Textarea', 'Select', 'Button', 'ImageButton',
                       'HiddenField']},
            '/',
            {'name': 'basicstyles',
             'items': ['Bold', 'Italic', 'Underline', 'Strike', 'Subscript', 'Superscript', '-', 'RemoveFormat']},
            {'name': 'paragraph',
             'items': ['NumberedList', 'BulletedList', '-', 'Outdent', 'Indent', '-', 'Blockquote', 'CreateDiv', '-',
                       'JustifyLeft', 'JustifyCenter', 'JustifyRight', 'JustifyBlock', '-', 'BidiLtr', 'BidiRtl',
                       'Language']},
            {'name': 'links', 'items': ['Link', 'Unlink', 'Anchor']},
            {'name': 'insert',
             'items': ['Image', 'Flash', 'Table', 'HorizontalRule', 'Smiley', 'SpecialChar', 'PageBreak', 'Iframe']},
            '/',
            {'name': 'styles', 'items': ['Styles', 'Format', 'Font', 'FontSize']},
            {'name': 'colors', 'items': ['TextColor', 'BGColor']},
            {'name': 'tools', 'items': ['Maximize', 'ShowBlocks']},
            {'name': 'about', 'items': ['About']},
            '/',  # put this to force next toolbar on new line
            {'name': 'yourcustomtools', 'items': [
                # put the name of your editor.ui.addButton here
                'Preview',
                'Maximize',

            ]},
        ],
        'toolbar': 'YourCustomToolbarConfig',  # put selected toolbar config here
        # 'toolbarGroups': [{ 'name': 'document', 'groups': [ 'mode', 'document', 'doctools' ] }],
        # 'height': 291,
        # 'width': '100%',
        # 'filebrowserWindowHeight': 725,
        # 'filebrowserWindowWidth': 940,
        # 'toolbarCanCollapse': True,
        # 'mathJaxLib': '//cdn.mathjax.org/mathjax/2.2-latest/MathJax.js?config=TeX-AMS_HTML',
        'tabSpaces': 4,
        'extraPlugins': ','.join([
            'uploadimage', # the upload image feature
            # your extra plugins here
            'div',
            'autolink',
            'autoembed',
            'embedsemantic',
            'autogrow',
            # 'devtools',
            'widget',
            'lineutils',
            'clipboard',
            'dialog',
            'dialogui',
            'elementspath'
        ]),
    }
}
```

