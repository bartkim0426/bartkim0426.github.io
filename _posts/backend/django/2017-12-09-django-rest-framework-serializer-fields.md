---
layout: post
title:  "Django serializer doc - serializer field"
categories: drf
---


# [Serializer fields](http://www.django-rest-framework.org/api-guide/fields/#serializer-fields)

serializer 필드는 primitive value와 internal datatypes간의 변환을 핸들링. 

또한 input value에 대한 validating도 해줌.

> Serializer field들은 `fields.py`에 선언되어 있는데, 편의를 위해서 
>
> `from rest_framework import serializers` 로 호출 후,
>
> `serializers.<FieldName>`로 사용하면 됨

## 1. [Core arguments](http://www.django-rest-framework.org/api-guide/fields/#core-arguments)

각각의 serializer field class는 적어도 3개의 args를 받음. 몇몇 Field는 추가적인 (field-specific) args가 필요

다음에 나오는 것들은 항상 accepted



### [`read_only`](http://www.django-rest-framework.org/api-guide/fields/#read_only)

API output에는 포함되고 input에는 포함되지 않음. 만약 `read_only` 된 필드가 포함되면 serializer가 무시함

default는 `False` 

### [`write_only`](http://www.django-rest-framework.org/api-guide/fields/#write_only)

default=False, True로 되어있으면 인스턴스를 updating, creating 할 때에는 사용되지만 serializing 할때는 포함되지 않음

### [`required`](http://www.django-rest-framework.org/api-guide/fields/#required)

default=True, 

deserialization 할 때 포함되어있지 않으면 에러 반환. deserialization시 필수가 아니면 `False`로

`False`로 해놓으면 인스턴스를 serializng attribute나 dict key를 누락시킴 => outut에서 포함되지 않음

### [`allow_null`](http://www.django-rest-framework.org/api-guide/fields/#allow_null)

default=False,

보통 `None`이 serializer 필드에 오면 에러 반환. `True`로 하면 `None`도 valid value로 간주

### [`default`](http://www.django-rest-framework.org/api-guide/fields/#default)

만약 세팅되면, input 값이 없을 때 해당 default 값을 투입. 

Partial update operation의 경우에는 default 값이 적용되지 않음. 

`default` value를 세팅한다는 것은 필드가 not required라는 것을 함축한다. `default`와 `required`를 모두 사용하면 에러 발생

### [`source`](http://www.django-rest-framework.org/api-guide/fields/#source)

populate 할 때 사용되는 attribute name.

 `self` arg를 받는 유일한 메쏘드 : `URLField(source='get_absolute_url')`

dotted notation을 사용: `EmailField(source='user.email')` => 만약 dotted notation을 사용하면 `default` value가 필수

`source='*'` 는 특별한 뜻을 가짐: object 전체가 이 필드를 거침(?): nested representation을 만들거나 전체 object에 접근이 필요할 때 유용함

### [`validators`](http://www.django-rest-framework.org/api-guide/fields/#validators)

field input에 적용될 validator function의 리스트. 

### [`error_messages`](http://www.django-rest-framework.org/api-guide/fields/#error_messages)

에러 코드-에러 메세지 dictionary

### [`label`](http://www.django-rest-framework.org/api-guide/fields/#label)

HTML form 필드나 다른 descriptive element들에 사용되는 짧은 text string

### [`help_text`](http://www.django-rest-framework.org/api-guide/fields/#help_text)

### [`initial`](http://www.django-rest-framework.org/api-guide/fields/#initial)

pre-populating value

```Python
import datetime
from rest_framework import serializers
class ExampleSerializer(serializers.Serializer):
    day = serializers.DateField(initial=datetime.date.today)
```

### [`style`](http://www.django-rest-framework.org/api-guide/fields/#style)

필드가 render 될 때 사용

```python
# Use <input type="password"> for the input.
password = serializers.CharField(
    style={'input_type': 'password'}
)

# Use a radio input instead of a select input.
color_channel = serializers.ChoiceField(
    choices=['red', 'green', 'blue'],
    style={'base_template': 'radio.html'}
)
```

자세한 내용은 [HTML & Forms](http://www.django-rest-framework.org/topics/html-and-forms/) 참조



# [Boolean fields](http://www.django-rest-framework.org/api-guide/fields/#boolean-fields)

## [BooleanField](http://www.django-rest-framework.org/api-guide/fields/#booleanfield)

default=True, django의 `django.db.models.fields.BooleanField`와 짝이맞음

## [NullBooleanField](http://www.django-rest-framework.org/api-guide/fields/#nullbooleanfield)

`None`을 valid value로 받는 booleaField





# [String fields](http://www.django-rest-framework.org/api-guide/fields/#string-fields)

## [CharField](http://www.django-rest-framework.org/api-guide/fields/#charfield)

validate - `max_length` ~ `min_length`

`CharField(max_length=None, min_length=None, allow_blank=False, trim_whitespace=True)`

- `max_length` : 최대길이
- `min_length` : 최소길이
- `allow_blank` : balnk=True, default는 False
- `trim_whitespace`: `True`면 앞/뒤의 whitespace를 trim (default가 True)

`allow_null` 옵션도 쓸 수 있지만 `allow_blank`를 쓰는걸 권장. 둘 다 하면 empty value로 두가지가 가능 => 버그 발생 가능성 증가

## [EmailField](http://www.django-rest-framework.org/api-guide/fields/#emailfield)

valid한 email address를 validate

`EmailField(max_length=None, min_length=None, allow_blank=False)`

## [RegexField](http://www.django-rest-framework.org/api-guide/fields/#regexfield)

**Signature:** `RegexField(regex, max_length=None, min_length=None, allow_blank=False)`

필수 arg: `regex` - string이나 컴파일된 python regular expression object도 가능

validation을 위해서는 `django.core.validators.RegexValidator`  사용

## [SlugField](http://www.django-rest-framework.org/api-guide/fields/#slugfield)

`[a-zA-Z0-9_-]+ ` 패턴을 validate 하는 `RegexField` 

**Signature:** `SlugField(max_length=50, min_length=None, allow_blank=False)`

## [URLField](http://www.django-rest-framework.org/api-guide/fields/#urlfield)

URL 매칭 패턴을 사용하는 `RegexField`

`http://<host>/<path>` 폼을 사용

**Signature:** `URLField(max_length=200, min_length=None, allow_blank=False)`

## [UUIDField](http://www.django-rest-framework.org/api-guide/fields/#uuidfield)

input에 valid한 UUID 스트링. `to_internal_value` 메서드를 쓰면 `uuid.UUID` 인스턴스 반환. => 정해진 하이픈 포멧이 나옴 (ex - "de305d54-75b4-431b-adb2-eb6b9e546013")

> UUID? 범용 고유 식별자 (universally unique identifier)
>
> 총 36개 문자로 된 8-4-4-4-12 5개 그룹으로 구성된 16옥탯의 수, 네트워크 상에서 모르는 개체들을 식별하고 구분하기 위한 각각의 고유한 이름 (출처-[위키](https://ko.wikipedia.org/wiki/%EB%B2%94%EC%9A%A9_%EA%B3%A0%EC%9C%A0_%EC%8B%9D%EB%B3%84%EC%9E%90))

Signature: `UUIDField(format='hex_verbose')`

- format: uuid 값의 포맷 결정: 나중에 사용할때 한번 읽어보면 좋을듯.
  - `'hex_verbose'` - The cannoncical hex representation, including hyphens: `"5ce0e9a5-5ffa-654b-cee0-1238041fb31a"`
  - `'hex'` - The compact hex representation of the UUID, not including hyphens: `"5ce0e9a55ffa654bcee01238041fb31a"`
  - `'int'` - A 128 bit integer representation of the UUID: `"123456789012312313134124512351145145114"`
  - `'urn'` - RFC 4122 URN representation of the UUID: `"urn:uuid:5ce0e9a5-5ffa-654b-cee0-1238041fb31a"` Changing the `format` parameters only affects representation values. All formats are accepted by `to_internal_value`

## [FilePathField](http://www.django-rest-framework.org/api-guide/fields/#filepathfield)

특정 dir타 파일시스템, 파일명으로 제한된 필드 (쓸일이 있나 싶긴함)

**Signature:** `FilePathField(path, match=None, recursive=False, allow_files=True, allow_folders=False, required=None, **kwargs)`

- `path` : dir의 절대경로. 
- `match`: 정규표현식 (str) -> filename을 찾는 필터 역할
- `recursize`: subdirectory 포함유무 (default=False)
- `allow_files`: 파일 포함 유무, default=True, `allow_files`나 `allow_folders` 중 하나는 True여야함
- `allow_folder`: 폴더 포함유무, default=False

## [IPAddressField](http://www.django-rest-framework.org/api-guide/fields/#ipaddressfield)

valid IPv4, IPv6 string

**Signature**: `IPAddressField(protocol='both', unpack_ipv4=False, **options)`

- `protocol` : 'both' (default), 'IPv4', 'IPv6' (case sensitive)
- `unpack_ipv4`: both일 경우에만 사용 가능, IPv4를 ::ffff:192.0.2.1 형태로 매핑?? 뭔말인지 잘

# [Numeric fields](http://www.django-rest-framework.org/api-guide/fields/#numeric-fields)

## [IntegerField](http://www.django-rest-framework.org/api-guide/fields/#integerfield)

**Signature**: `IntegerField(max_value=None, min_value=None)`

- `max_value` Validate that the number provided is no greater than this value.
- `min_value` Validate that the number provided is no less than this value.

## [FloatField](http://www.django-rest-framework.org/api-guide/fields/#floatfield)

**Signature**: `FloatField(max_value=None, min_value=None)`

## [DecimalField](http://www.django-rest-framework.org/api-guide/fields/#decimalfield)

파이썬의 `Decimal` 인스턴스로 대변되는 필드

사용할 일이 있을 때 문서 참고해서 읽은 뒤 사용하면 될듯.. 

> Decimal? 부동소수점 연산의 본질적인 문제를 해결 가능. 
>
> float과 다르게 실수 정확하게 표현, Infinity, -Infinity, NaN 표현 가능 (참고: [네이버 블로그](https://m.blog.naver.com/PostView.nhn?blogId=dudwo567890&logNo=130165293039&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F))

**Signature**: `DecimalField(max_digits, decimal_places, coerce_to_string=None, max_value=None, min_value=None)`

- `max_digits`: The maximum number of digits allowed in the number. `None`이나 `decimal_places`의 숫자보다 크거나 같아야함. 
- `decimal_places`: The number of decimal places to store with the number.
- `coerce_to_string` Set to `True` if string values should be returned for the representation, or `False` if `Decimal` objects should be returned. Defaults to the same value as the `COERCE_DECIMAL_TO_STRING`settings key, which will be `True` unless overridden. If `Decimal` objects are returned by the serializer, then the final output format will be determined by the renderer. Note that setting `localize` will force the value to `True`.
- `max_value` Validate that the number provided is no greater than this value.
- `min_value` Validate that the number provided is no less than this value.
- `localize` Set to `True` to enable localization of input and output based on the current locale. This will also force `coerce_to_string` to `True`. Defaults to `False`. Note that data formatting is enabled if you have set `USE_L10N=True` in your settings file.
- `rounding` Sets the rounding mode used when quantising to the configured precision. Valid values are [`decimal` module rounding modes](https://docs.python.org/3/library/decimal.html#rounding-modes). Defaults to `None`.



# [Date and time fields](http://www.django-rest-framework.org/api-guide/fields/#date-and-time-fields)

## [DateTimeField](http://www.django-rest-framework.org/api-guide/fields/#datetimefield)

django의 `DateTimeField` 

**Signature:** `DateTimeField(format=api_settings.DATETIME_FORMAT, input_formats=None)`

- `format` : default는 setting에서 정의된 `DATETIME_FORMAT` 과 동일 (`iso-8601`) 
- `input_formats`: date를 parse할 때 사용하는 input format들의 리스트. 지정 안하면 `DATETIME_INPUT_FORMATS` 세팅이 사용 => `[iso-8601]`

> 포멧 스트링은 [Python strftime formats](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior) 이나 [ISO 8601](http://www.w3.org/TR/NOTE-datetime) style(`'2013-01-29T12:34:56.000000Z'`) 어떤것도 상관 없음

`ModelSerializer`를 사용하는 경우, model field에서 `auto_now_add=True`나 `auto_add=True`로 되어 있다면 `read_only=True`를 자동으로 생성함. 이를 override 하고 싶다면 serializer에서 명시적으로 다시 선언해야함. 

ex)

```python
class CommentSerializer(serializers.ModelSerializer):
    created = serializers.DateTimeField()

    class Meta:
        model = Comment
```

## [DateField](http://www.django-rest-framework.org/api-guide/fields/#datefield)

**Signature:** `DateField(format=api_settings.DATE_FORMAT, input_formats=None)`

위랑 동일.  ISO 8601 형태는 `'2013-01-29'`

## [TimeField](http://www.django-rest-framework.org/api-guide/fields/#timefield)

**Signature:** `TimeField(format=api_settings.TIME_FORMAT, input_formats=None)`

이것도 위랑 동일. ISO 8601 형태는 `'12:34:56:000000'`

## [DurationField](http://www.django-rest-framework.org/api-guide/fields/#durationfield)

`validate_data` 는 `datetime.timedelta` 인스턴스를 가짐 => `'[DD][]'HH:[MM:]]ss[.uuuuuu]'` 형태

> django >= 1.8 이상에서 사용 가능



# [Choice selection fields](http://www.django-rest-framework.org/api-guide/fields/#choice-selection-fields)

## [ChoiceField](http://www.django-rest-framework.org/api-guide/fields/#choicefield)

set of choices의 value를 받을 수 있는 필드

`choices-...` arg를 가진 `ModelSerializer`를 사용하면 자동으로 생성됨

**Signature:** `ChoiceField(choices)`

- `choices`: `(key, display_name)` 튜플의 리스트
- `allow_blank`: default는 False, `True`면 empty string도 valid value로 취급됨. fales면 validation error
- `html_cutoff` : default=`None` , 만약 세팅되면 HTML 셀렉트 드롭다운에서 나타나는 최대 숫자 조절 가능
- `html_cutoff_text` :  If set this will display a textual indicator if the maximum number of items have been cutoff in an HTML select drop down. Defaults to `"More than {count} items…"` (무슨말이지)

`allow_blank`, `allow_null` 둘 다 사용 가능하지만 둘다 쓰지 말고 한개만 쓸것을 강력하게 추천. 

`allow_blank`는 textural choices에, `allow_null`은 numeric, non-textual choices에 쓰길

## [MultipleChoiceField](http://www.django-rest-framework.org/api-guide/fields/#multiplechoicefield)

**Signature:** `MultipleChoiceField(choices)`

`ChoiceField`랑 동일하게 사용, `to_internal_value` 는 선택된 value의 set을 반환





# [File upload fields](http://www.django-rest-framework.org/api-guide/fields/#file-upload-fields)

 `MultiPartParser` or `FileUploadParser` 를 사용해야만 `FileField`, `ImageField` 가 맞음

JSON 등의 parser는 file upload 지원 안함 (그럼 REST에서 파일 업로드는 어떤식으로 하는지?? )



## [FileField](http://www.django-rest-framework.org/api-guide/fields/#filefield)

**Signature:** `FileField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL)`

- `max_length` : 파일명 최대 길이
- `allow_empty_file` : 빈파일 허용하는지
- `use_url`: `True`라면 URL string value가 output에서 사용됨. `False`면 파일명이 사용. default는 `UPLOAD_FILES_USE_URL` 세팅값이랑 동일 (`True`가 기본형)

## [ImageField](http://www.django-rest-framework.org/api-guide/fields/#imagefield)

**Signature:** `ImageField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL)`

filefield랑 동일. 

`Pillow`나 `PIL` 패키지 필요. (PIL은 유지보수 안되니까 Pillow 추천)





# [Composite fields](http://www.django-rest-framework.org/api-guide/fields/#composite-fields)

## [ListField](http://www.django-rest-framework.org/api-guide/fields/#listfield)

List 오브젝트를 validate하는 필드

**Signature**: `ListField(child=<A_FIELD_INSTANCE>, min_length=None, max_length=None)`

- `child` : list 안의 object들을 validating 하는 필드 인스턴스. 없으면 validating 하지 않음
- `min_length` : list의 object 최소 개수
- `max_length`: list의 object 최대 갯수

i.e.

```python
scores = serializers.ListField(
   child=serializers.IntegerField(min_value=0, max_value=100)
)
```

`ListField`는 재사용 가능한 선언형 스타일도 지원

```python
class StringListField(serializers.ListField):
    child = serializers.CharField()
```

=> `StringListField`를 사용 가능 (child 제공)

## [DictField](http://www.django-rest-framework.org/api-guide/fields/#dictfield)

dict object를 validate 하는 필드.

**Signature**: `DictField(child=<A_FIELD_INSTANCE>)`

- `child`: 위의 `ListField`와 동일한 형태

## [JSONField](http://www.django-rest-framework.org/api-guide/fields/#jsonfield)

유효한 JSON data를 validate하는 필드

**Signature**: `JSONField(binary)`

- `binary` : True면 output으로 validate JSON encoded string을 반환. default는 False

# [Miscellaneous fields](http://www.django-rest-framework.org/api-guide/fields/#miscellaneous-fields)

## [ReadOnlyField](http://www.django-rest-framework.org/api-guide/fields/#readonlyfield)

## [HiddenField](http://www.django-rest-framework.org/api-guide/fields/#hiddenfield)

user input data가 아닌 default value나 callableㅇ르 받는 필드

i.e. 

```python
modified = serializers.HiddenField(default=timezone.now)
```

## [ModelField](http://www.django-rest-framework.org/api-guide/fields/#modelfield)

model field와 매칭 가능한 필드. 

**Signature:** `ModelField(model_field=<Django ModelField instance>)`

## [SerializerMethodField](http://www.django-rest-framework.org/api-guide/fields/#serializermethodfield)

read-only field. 

```python
from django.contrib.auth.models import User
from django.utils.timezone import now
from rest_framework import serializers

class UserSerializer(serializers.ModelSerializer):
    days_since_joined = serializers.SerializerMethodField()

    class Meta:
        model = User

    def get_days_since_joined(self, obj):
        return (now() - obj.date_joined).days
```



# [Custom fields](http://www.django-rest-framework.org/api-guide/fields/#custom-fields)

custom field를 만드려면, `.to_representation()` 이나 `.to_internal_value()` 를 override 해야함 (둘중 하나나 둘 다)

Initial datatype과 serializable datatype간의 coverting할 때 사용

`.to_representation()` : initial datatype을 primitive, serializable datatype으로 변환할때 사용

`to_internal_value()` : primitive datatype을 internal python representation으로 복구할때 사용. data가 invalid하면 `serializers.ValidationError`

i.e.

```python
class Color(object):
    """
    A color represented in the RGB colorspace.
    """
    def __init__(self, red, green, blue):
        assert(red >= 0 and green >= 0 and blue >= 0)
        assert(red < 256 and green < 256 and blue < 256)
        self.red, self.green, self.blue = red, green, blue

class ColorField(serializers.Field):
    """
    Color objects are serialized into 'rgb(#, #, #)' notation.
    """
    def to_representation(self, obj):
        return "rgb(%d, %d, %d)" % (obj.red, obj.green, obj.blue)

    def to_internal_value(self, data):
        data = data.strip('rgb(').rstrip(')')
        red, green, blue = [int(col) for col in data.split(',')]
        return Color(red, green, blue)
```

- Raising validation errors

```python
def to_internal_value(self, data):
    if not isinstance(data, six.text_type):
        msg = 'Incorrect type. Expected a string, but got %s'
        raise ValidationError(msg % type(data).__name__)

    if not re.match(r'^rgb\([0-9]+,[0-9]+,[0-9]+\)$', data):
        raise ValidationError('Incorrect format. Expected `rgb(#,#,#)`.')

    data = data.strip('rgb(').rstrip(')')
    red, green, blue = [int(col) for col in data.split(',')]

    if any([col > 255 or col < 0 for col in (red, green, blue)]):
        raise ValidationError('Value out of range. Must be between 0 and 255.')

    return Color(red, green, blue)
```

`ValidationError` 의 shortcut으로 `.fail()` 메서드도 사용 가능 (`error_messages` 딕트의 메세지 스트링을 받음)

```python
default_error_messages = {
    'incorrect_type': 'Incorrect type. Expected a string, but got {input_type}',
    'incorrect_format': 'Incorrect format. Expected `rgb(#,#,#)`.',
    'out_of_range': 'Value out of range. Must be between 0 and 255.'
}

def to_internal_value(self, data):
    if not isinstance(data, six.text_type):
        self.fail('incorrect_type', input_type=type(data).__name__)

    if not re.match(r'^rgb\([0-9]+,[0-9]+,[0-9]+\)$', data):
        self.fail('incorrect_format')

    data = data.strip('rgb(').rstrip(')')
    red, green, blue = [int(col) for col in data.split(',')]

    if any([col > 255 or col < 0 for col in (red, green, blue)]):
        self.fail('out_of_range')

    return Color(red, green, blue)
```



# [Third party packages](http://www.django-rest-framework.org/api-guide/fields/#third-party-packages)

이런게 있다 정도..만 알고 필요할 때 찾아 쓰면 될듯

## [DRF Compound Fields](http://www.django-rest-framework.org/api-guide/fields/#drf-compound-fields)

The [drf-compound-fields](https://drf-compound-fields.readthedocs.io/) package provides "compound" serializer fields, such as lists of simple values, which can be described by other fields rather than serializers with the `many=True` option. Also provided are fields for typed dictionaries and values that can be either a specific type or a list of items of that type.

## [DRF Extra Fields](http://www.django-rest-framework.org/api-guide/fields/#drf-extra-fields)

The [drf-extra-fields](https://github.com/Hipo/drf-extra-fields) package provides extra serializer fields for REST framework, including `Base64ImageField` and `PointField` classes.

## [djangrestframework-recursive](http://www.django-rest-framework.org/api-guide/fields/#djangrestframework-recursive)

the [djangorestframework-recursive](https://github.com/heywbj/django-rest-framework-recursive) package provides a `RecursiveField` for serializing and deserializing recursive structures

## [django-rest-framework-gis](http://www.django-rest-framework.org/api-guide/fields/#django-rest-framework-gis)

The [django-rest-framework-gis](https://github.com/djangonauts/django-rest-framework-gis) package provides geographic addons for django rest framework like a`GeometryField` field and a GeoJSON serializer.

## [django-rest-framework-hstore](http://www.django-rest-framework.org/api-guide/fields/#django-rest-framework-hstore)

The [django-rest-framework-hstore](https://github.com/djangonauts/django-rest-framework-hstore) package provides an `HStoreField` to support [django-hstore](https://github.com/djangonauts/django-hstore)`DictionaryField` model field.
