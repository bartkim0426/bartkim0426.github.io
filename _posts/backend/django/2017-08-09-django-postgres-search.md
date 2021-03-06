---
layout: post
title:  "django postgres full text search"
categories: django postgres
---

## django full text search
[django full text search document](https://docs.djangoproject.com/en/1.10/ref/contrib/postgres/search/#trigramsimilarity)

[django postgresql specific lookups](https://docs.djangoproject.com/en/1.10/ref/contrib/postgres/lookups/#std:fieldlookup-trigram_similar)

[stackoverflow example using TrigramSimilarity](https://stackoverflow.com/questions/37859961/combine-trigram-with-ranked-searching-in-django-1-10)


### Before using
add settings.py `django.contrib.postgres`


### Install `pg_trgm.sql` in server
`psql -f /usr/share/postgresql/8.4/contrib/pg_trgm.sql`


[psql 문서](https://www.postgresql.org/docs/current/static/pgtrgm.html) 참고

### Use by GET request
```
    def get_queryset(self):
        queryset = super(FilterMixin, self).get_queryset().filter(**self.get_queryset_filters())
        searching = self.request.GET.get("searching", "")
        if searching != "":
            queryset = queryset.annotate(
                similarity=TrigramSimilarity(
                    'title', searching
                ) + TrigramSimilarity(
                    'country__name', searching
                ) + TrigramSimilarity(
                    'country__region__name', searching
                ) + TrigramSimilarity(
                    'subject__name', searching
                ) + TrigramSimilarity(
                    'knowledge_type__name', searching
                )
            ).filter(similarity__gt=0.3).order_by('-similarity')
        else:
            pass
```
