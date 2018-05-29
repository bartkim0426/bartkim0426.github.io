---
layout: post
title:  "django 어드민을 잘쓰자! django admin cookbook"
categories: django
---


`django`를 사용하면서 가장 좋다고 생각했던 것 중 하나는 `admin`이다. 기본 admin도 훌륭할 뿐 아니라 suit, jet 등 다른 라이브러리를 붙여서 사용하면 매우 강력하노 훌륭한 admin 사이트를 기본으로 사용 가능하게 된다.

기본적으로 `django admin`은 아주 다양하고 폭넓은 기능을 제공한다. 기본으로 제공하는 기능이 너무 많기 때문에 이를 잘 찾아서 사용하기가 쉽지 않고, 장곡 공식 문서도 방대해서 원하는 기능을 찾아내기가 쉽지 않았는데 어드민에서 사용할 수 있는 다양한 기능과 방법을 잘 정리해 놓은 사이트가 있어서 공유한다.

[`django admin cookbook`](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/imagefield.html)이라는 곳에서 장고 어드민을 사용하는 팁, 자주 찾아보게 되는 것을을 잘 정리해 두었다.

일단 목차정도만 간략하게 번역하고, 시간이 되면 항목별로 조금씩 정리를 해서 많은 장고 사용자들에게 도움이 되었으면 한다.

자세히 읽지는 않더라도, 이런 기능들이 있다는 것들만 알고 있다면 어드민을 만들다가 필요한 기능이 생겼을 때 쉽게 찾아서 사용이 가능할것같다.


### 1. `Django administration` 기본 text 변경하는방법
```python
admin.site.site_header = "UMSRA Admin"
admin.site.site_title = "UMSRA Admin Portal"
admin.site.index_title = "Welcome to UMSRA Researcher Portal"
```

### 2. 모델의 복수형 (Posts, Users) 바꾸는법
한글로 admin을 만들 경우에는 필수로 `verbose_name`과 `verbose_name_plural`을 수정해주는게 좋다.
```python
class Category(models.Model):
    ...

    class Meta:
        verbose_name_plural = "Categories"


class Hero(Entity):
    ...

    class Meta:
        verbose_name_plural = "Heroes"
```

### 3. [두개의 별도 admin 사이트를 만드는 방법?](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/two_admin.html)


### 4. [기본 앱을 admin에서 삭제하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/remove_default.html)

### 5. [django admin에 로고 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/logo.html)

### 6. [django admin 템플릿 덮어쓰기](https://docs.djangoproject.com/en/dev/ref/contrib/admin/#overriding-admin-templates)

### 7. [listview page(모델 리스트 보느 페이지)에서 계산된 값 넣기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/calculated_fields.html)

### 8. [django admin에서 쿼리 최적화하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/optimize_queries.html)

### 9. [계산된 필드로 sorting 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/sorting_calculated_fields.html)

### 10. [계산된 필드 필터 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/filtering_calculated_fields.html)

### 11. [특정 계산값이나 boolean값으로 on/off 아이콘 뿌려주기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/boolean_fields.html)

### 12. [추가적인 actions (삭제 액션같은) 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/add_actions.html)

### 13. [django admin에서 csv 파일 추출하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/export.html)

### 14. [삭제 액션 없애기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/remove_delete_selected.html)

### 15. [리스트 페이지에 액션이 아닌 커스텀 버튼 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/action_buttons.html)

### 16. [django admin을 사용해서 csv 파일 import하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/action_buttons.html)

### 17. [특정 user에게 admin 페이지 제한하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/action_buttons.html)

### 18. [admin의 특정 부분만 제한하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/restrict_parts.html)

### 19. [한개의 object만 생성 가능하게 하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/only_one.html)

### 20. ['추가/변경' 버튼 제거하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/remove_add_delete.html)

### 21. [한 어드민에서 여러 모델을 편집하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/edit_multiple_models.html)

### 22. [admin 인라인에서 one to one 모델 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/edit_multiple_models.html)

### 23. [중첩된 inlines 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/nested_inlines.html)

### 24. [다른 두개의 모델로 한개의 어드민 만들기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/single_admin_multiple_models.html)

### 25. [listview 페이지에서 더 많은 칼럼 보기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/increase_row_count.html)

### 26. [admin 페이지네이션 disable](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/disable_pagination.html)

### 27. [date를 기준으로 필터링하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/date_based_filtering.html)

### 28. [listview page에서 Many to many / reverse foreignkey 필드 보기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/many_to_many.html)

### 29. [imagefield의 이미지 보여주기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/imagefield.html)

### 30. [모델 저장시 현재 user와 연결하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/current_user.html)

### 31. [Readonly field로 만들기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/changeview_readonly.html)

### 32. [수정 불가능한 필드 (created_at 등) 어드민에서 보기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/uneditable_field.html)

### 33. [생성시에만 수정 가능하게 하고 변경시에는 수정 불가능하게 하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/uneditable_existing.html)

### 34. [ForeignKey 드랍다운 필터하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/filter_fk_dropdown.html)

### 35. [많은 수의 foreignkey object 다루기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/many_fks.html)

### 36. [Foreignkey 드랍다운에 text 변경하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/fk_display.html)

### 37. [admin change view page (모델 object 수정 페이지)에 커스텀 버튼 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/custom_button.html)

### 38. [특정 object의 admin url 구하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/object_url.html)

### 39. [django admin에서 같은 모델 어드민을 두번 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/add_model_twice.html)

### 40. [django admin의 save method를 override 하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/override_save.html)

### 41. [어드민에서 database view를 추가하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/database_view.html)

### 42. [어드민 dashboared에서 앱 순서 변경하기](http://books.agiliq.com/projects/django-admin-cookbook/en/latest/set_ordering.html)
