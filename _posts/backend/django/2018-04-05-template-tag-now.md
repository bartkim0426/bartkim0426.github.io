---
layout: post
title:  "Django: template tag 'now'"
categories: django
---


When want to use `now` date or datetime in django temlpate, you can pass `datetime.now()` for context data.

But you can simply use `now` template tag. It displays the current date and/or time, using a format.

{% raw %}
```template
<p>
    the time is {% now "jS F Y H:i" %}
</p>
```
{% endraw %}

You can pass predefined ones : `DATE_FORMAT`, `DATETIME_FORMAT`, `SHORT_DATE_FORMAT`, `SHORT_DATETIME_FORMAT` like this.

{% raw %}
```template
<p>
    it is {% now "SHORT_DATE_FORMAT" %}
</p>
```
{% endraw %}


Also you can use syntax like `{% raw %}{% now 'Y' as current_year %}{% endraw %}`. 

{% raw %}
```template
{% now "Y" as current_year %}
{% blocktrans %}Copyright {{ current_year }}{% endblocktrans %}
```
{% endraw %}
