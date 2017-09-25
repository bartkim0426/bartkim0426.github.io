---
layout: post
title:  "using SOX(Sound eXchange) and ffmpeg on mac"
categories: api
---

## Installing Sox
download zip file from [Sox page](https://sourceforge.net/projects/sox/?source=typ_redirect) and unzip it.    

After unzip, register PATH in `~/.zshrc`(or `~/.bashrc`)    
`export PATH=$PATH:/path/to/sox-dir`    
Then you can use sox commands like `sox`, `play`.

## Installing ffmpeg
`brew install ffmpeg`  
> [blog](http://angrygoguma.tistory.com/117)


### Changing Stereo to Mono
If you want to use google API, you have to change stereo to mono.    
```bash
sox input.flac outfile.l.flac remix 1 
sox input.flac outfile.l.flac remix 2
```

### Changing mp3 to flac    
```bash
# using ffmpeg
ffmpeg -i input.mp3 output.flac

# using sox
sox audio.wav --channels=1 --bits=16 audio.flac
sox audio.ogg --channels=1 --bits=16 audio.flac
sox audio.au --channels=1 --bits=16 audio.flac
sox audio.aiff --channels=1 --bits=16 audio.flac

```

### Changing FLAC file to LINEAR16
`sox example.flac example.raw`

### Changing sampling rate
`sox -r 44.1k -e signed -c 2 -b 16 input.wav output.wav`

> [good webpage](http://archive.oreilly.com/pub/post/convert_audio_between_mp3_flac.html) converting audio format

