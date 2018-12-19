---
layout: post
title:  "python: package python project"
categories: python
---


## 1. Creating setup.py

다양한 정보들을 포함시킨 `setup.py` 파일을 만든다. 해당 패키지의 버전, 이름 등을 알려준다.

```python
import setuptools

with open("README.md", "r") as fh:
    long_description = fh.read()

setuptools.setup(
    name="example_package",
    version="0.1.0",
    author="bartkim0426",
    author_email="bartkim0426@gmail.com",
    description="short description",
    long_description=long_description,
    long_description_content_type="text/markdown",
    packages=setuptools.find_packages(),
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
)
```


## 2. creating README.md & LICENSSE

- readme를 만들어 패키지에 대한 설명 및 사용법을 적어준다.
- 개인 프로젝트일 경우 라이센스를 생략하는 것이 많지만, 적어주는게 좋다. 라이센스 종류는 [choosealicense](https://choosealicense.com/)를 참고. 아래는 MIT라이센스


```
Copyright (c) 2018 The Python Packaging Authority

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```


## 3. setuptools, wheel 설치

- 패키지 distribution을 위한 setuptools, wheel 패키지를 설치한다.

```
python -m pip install --user --upgrade setuptools wheel
```


## 4. make distribution archives (dist)

- 이제 실제 배포를 위한 아카이브를 생성한다. 

```
python setup.py sdist bdist_wheel
```

생성 이후에 프로젝트 디렉토리에 `dist/` 디렉토리가 생긴것을 볼 수 있다.

## 5. pypi 계정 생성

- 업로드를 위해서는 [PYPI(Python Package Index)](https://pypi.org/)의 계정이 있어야 한다. 
- 이미 회원이라면 생략하고, 아니라면 [여기](https://pypi.org/)에 들어가서 회원가입과 메일 인증을 해준다.

## 5. `.pypirc` 생성하기

- 홈 디렉토리에 `.pypirc` 파일을 생성하고 계정정보를 적어준다. 적지 않아도 되지만 그러면 `twine`을 사용할 때 직접 서버, 계정정보 등을 적어주어야 하는 불편함이 있다.
- `touch ~/.pypirc`

```
[distutils]
index-servers =
    pypi

[pypi]
repository: https://upload.pypi.org/legacy/
username: bartkim0426
password: my_password
```


## 6. 업로드하기

- `twine` 패키지를 이용해서 `pypi`에 업로드가 가능하다.


```python
python -m pip install --user --upgrade twine
```

그리고 다음 명령어로 project directory의 `dist/` 아카이브를 업로드할 수 있다.

```python
twine upload dist/*
```

## 7. 내 패키지 불러와보기

- 업로드가 완료되면 pip를 사용해서 내 패키지를 받아서 사용 가능하다.

```
pip install example_package
```
