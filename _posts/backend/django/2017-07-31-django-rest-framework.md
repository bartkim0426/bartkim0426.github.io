---
layout: post
title:  "django rest framework"
categories: django
---

## [ Django Rest Framework ](http://www.django-rest-framework.org/#installation)

### Basic usages

**1. making `serializer.py`**


```python
from rest_framework import serializers
from ..models import Artwork


class ArtworkSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
```

**2. using in shell**

```shell
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser

# making var
artwork = Artwork.objects.last()

# make sr data
sr = ArtworkSerializer(artwork)
sr.data

content = JSONRenderer().render(sr.data)
content

# how to Deserialization
from django.utils.six import BytesIO

stream = BytesIO(content)
data = JSONParser().parse(stream)

# restore validated data
serializer = SnippetSerializer(data=data)
serializer.is_valid()
# True
serializer.validated_data
# OrderedDict([('title', ''), ('code', 'print "hello, world"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
serializer.save()
# <Snippet: Snippet object>


```
