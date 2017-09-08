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

## Using Sox    



### Changing mp3 to flac    
`ffmpeg -i input.mp3 output.flac`

> [good webpage](http://archive.oreilly.com/pub/post/convert_audio_between_mp3_flac.html) converting audio format
