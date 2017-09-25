---
layout: post
title:  "Django Tips"
categories: django
tags: django
---

## Django form에서 label 없애기
```
class SpeechForm(ModelForm):
    class Meta:
        model = Speech
        fields = ['file_name', 'title', 'lecture_date', 'memo']
    def __init__(self, *args, **kwargs):
        super(SpeechForm, self).__init__(*args, **kwargs)

        for fname, f in self.fields.items():
            f.label = ''
```
[SO](https://stackoverflow.com/questions/9332638/django-how-to-hide-overwrite-default-label-with-modelform)


