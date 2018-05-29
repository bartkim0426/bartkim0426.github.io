---
layout: post
title:  "Django: DetailView, UpdateView pk나 slug 없이 사용하기"
categories: django
---

CBV를 주로 사용하면서, 참 잘 만들었다고 감탄하고 있다.

DetailView, UpdateView 등 pk나 slug가 기본으로 필요한 CBV를 사용할 때, pk나 slug 없이 특정 object를 전달하는 방법을 공유한다.

`get_object`에 직접 해당 object를 넘겨주기만 하면 url에서 따로 pk, slug 사용 없이 깔끔하게 DetailView 등을 사용 가능하다.
user 기반으로 한 페이지 (user mypage 등)을 만들 때 유용하게 사용할 수 있다.

다음은 마이페이지에서 프로필 사진 업데이트를 할 수 있는 `MypageView`의 예.

```python
class MypageView(LoginRequiredMixin, UpdateView):
    model = User
    fields = ['profile_image']
    template_name = 'my_page.html'
    success_url = '/mypage'

    def get_object(self):
        return self.request.user
```
