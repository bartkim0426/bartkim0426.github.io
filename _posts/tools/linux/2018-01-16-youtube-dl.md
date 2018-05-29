---
layout: post
title:  "Downloading youtube in terminal: youtube-dl"
categories: linux
---

요즘은 유투브 영상을 쉽게 다운로드 받을 수 있는 다양한 프로그램, 익스텐션 등이 있어서 쉽게 영상을 다운받을 수 있다. 

터미널에서도 유투브 영상을 다운받을 수 있는 흥미로운 라이브러리가 있어서 소개해본다. 


이름은 [**youtube-dl**](https://github.com/rg3/youtube-dl/blob/master/README.md#installation).


## 설치
맥의 경우에는 `brew`, 리눅스는 `pip`로 설치할 수 있다. (물론 `curl`로 직접 다운받아 설치해도 된다.)


```bash
# mac os의 경우 brew 사용
$brew install youtube-dl

# pip로 설치
$ sudo -H pip install --upgrade youtube-dl

# 직접 설치
$ sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
$ sudo chmod a+rx /usr/local/bin/youtube-dl
```


## 사용법
`-F` 옵션으로 다운로드 가능한 정보들을 확인한 후 해당 옵션의 영상을 다운로드 받으면 된다. 

```bash
# check video info 
youtube-dl -F <url>

[youtube] f0i4_JEGwZ4: Downloading webpage
[youtube] f0i4_JEGwZ4: Downloading video info webpage
[youtube] f0i4_JEGwZ4: Extracting video information
[info] Available formats for f0i4_JEGwZ4:
format code  extension  resolution note
249          webm       audio only DASH audio   55k , opus @ 50k, 2.68MiB
250          webm       audio only DASH audio   73k , opus @ 70k, 3.48MiB
171          webm       audio only DASH audio  109k , vorbis@128k, 5.50MiB
140          m4a        audio only DASH audio  128k , m4a_dash container, mp4a.40.2@128k, 6.90MiB
251          webm       audio only DASH audio  144k , opus @160k, 6.92MiB
278          webm       256x144    144p  105k , webm container, vp9, 25fps, video only, 5.03MiB
160          mp4        256x144    144p  112k , avc1.4d400c, 25fps, video only, 4.35MiB
242          webm       426x240    240p  226k , vp9, 25fps, video only, 10.64MiB
133          mp4        426x240    240p  262k , avc1.4d4015, 25fps, video only, 7.16MiB
243          webm       640x360    360p  427k , vp9, 25fps, video only, 19.47MiB
134          mp4        640x360    360p  557k , avc1.4d401e, 25fps, video only, 14.72MiB
244          webm       854x480    480p  776k , vp9, 25fps, video only, 34.38MiB
135          mp4        854x480    480p 1129k , avc1.4d401e, 25fps, video only, 29.42MiB
247          webm       1280x720   720p 1553k , vp9, 25fps, video only, 68.84MiB
136          mp4        1280x720   720p 2268k , avc1.4d401f, 25fps, video only, 58.57MiB
17           3gp        176x144    small , mp4v.20.3, mp4a.40.2@ 24k
36           3gp        320x180    small , mp4v.20.3, mp4a.40.2
43           webm       640x360    medium , vp8.0, vorbis@128k
18           mp4        640x360    medium , avc1.42001E, mp4a.40.2@ 96k
22           mp4        1280x720   hd720 , avc1.64001F, mp4a.40.2@192k (best)
```

이후 `-f` 옵션을 주고 해당 정보를 입력하면 다운로드 받을 수 있다. 위의 옵션에서 `144k` 음질과 `1280*720` (best) 화질 옵션으로 다운로드 받아보자. 
```bash
youtube-dl -f 22+140 https://www.youtube.com/watch?v=f0i4_JEGwZ4
```


### 플레이리스트의 영상 모두 받기
```bash
youtube-dl -citw -f 22 <url>
```

### 자막 다운받기
```bash
--write-sub                      Write subtitle file
--write-auto-sub                 Write automatically generated subtitle file
                                 (YouTube only)
--all-subs                       Download all the available subtitles of the
                                 video
--list-subs                      List all available subtitles for the video
--sub-format FORMAT              Subtitle format, accepts formats
                                 preference, for example: "srt" or
                                 "ass/srt/best"
--sub-lang LANGS                 Languages of the subtitles to download
                                 (optional) separated by commas, use --list-
                                 subs for available language tags
```

### Auth option (비공개일경우)
```bash
-u, --username USERNAME          Login with this account ID
-p, --password PASSWORD          Account password. If this option is left
                                 out, youtube-dl will ask interactively.
-2, --twofactor TWOFACTOR        Two-factor authentication code
-n, --netrc                      Use .netrc authentication data
--video-password PASSWORD        Video password (vimeo, smotri, youku)
```


이 외에 아주 다양한 설정, 사용법이 있다. 더 자세하게 알고 싶으면 [youtube-dl 공식 깃헙](https://github.com/rg3/youtube-dl/blob/master/README.md#installation)을 참고하자!
