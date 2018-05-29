---
layout: post
title:  "Django: multiple pagination in CBV"
categories: django
---

When using CBV, especially `ListView`, it's really easy to make pagination, using `paginated_by`.


But what if you need more than one pagination, or pagination for not a `ListView` model objects? 


### In Views
We can use django `Paginator`. Just import `Paginator`, `PageNotAnInteger`, and `EmptyPage` and use it.

```python
from django.core.paginator import (
    Paginator,
    PageNotAnInteger,
    EmptyPage,
)
...


class MypageView(LoginRequiredMixin, UpdateView):
    model = User
    template_name = 'mypage/my_page.html'
    success_url = '/mypage'

    def get_object(self):
        return self.request.user

    def get_context_data(self, **kwargs):
        context = super(MypageView, self).get_context_data(**kwargs)
        jobs = Jobs.objects.filter(user=self.request.user)
        # Can make custom paginator!
        paginator = Paginator(jobs, 2)
        page = self.request.GET.get('page1')
        try:
            jobs = paginator.page(page)
        except PageNotAnInteger:
            jobs = paginator.page(1)
        except EmptyPage:
            jobs = paginator.page(paginator.num_pages)
    ...
```

### In Template

{% raw %}
```html
{% if jobs.has_previous %}
    <a href="?page1={{ jobs.previous_page_number }}"> previous </a>
{% endif %}
<span class="current">
    Page {{ jobs.number }} of {{ jobs.paginator.num_pages }}
</span>
{% if jobs.has_next %}
    <a href="?page1={{ jobs.next_page_number }}"> next </a>
{% endif %
```
{% endraw %}
