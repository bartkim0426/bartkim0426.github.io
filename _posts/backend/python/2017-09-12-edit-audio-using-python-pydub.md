---
layout: post
title:  "Edit Audio file using python- pydub"
categories: python pydub
---

## Intro
파이썬으로 audio 파일을 수정해야 할 일이 생겨 여러 라이브러리를 찾아 보았다. `Python Audio Tools`, `pydub` 등 여러 가지 라이브러리 중 가장 github star가 많고, 아직까지 상대적으로 활동이 활발한 pydub를 사용하기로 하였다.    

[깃헙 홈페이지](https://github.com/jiaaro/pydub)    
[공식 홈페이지](http://pydub.com/)

## Installing
`pip install pydub`로 간단히 설치 하거나 [공식 홈페이지](http://pydub.com/)에서 다운받아 설치 가능하다.

## Basic usage
```python
from pydub import AudioSegment

# Open file
song = AudioSegment.from_mp3('song.mp3')

# Slice audio
# pydub는 milliseconds 단위를 사용한다
ten_seconds = 10 * 1000
one_min = ten_seconds * 6

first_10_seconds = song[:ten_seconds]
last_5_seconds = song[-5000:]

# up/down volumn
beginning = first_10_seconds + 6

# Save the result
# can give parameters-quality, channel, etc
beginning.exoprt('result.flac', format='flac', parameters=["-q:a", "10", "-ac", "1"])
```

### Change stereo to mono
```python
from pydub import AudioSegment

sound = AudioSegment.from_wav("/path/to/file.wav")
sound = sound.set_channels(1)
sound.export("/output/path.wav", format="wav")
```
