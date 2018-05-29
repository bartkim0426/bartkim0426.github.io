---
layout: post 
title:  "맥북 디스크 타입 확인하기"
categories: mac
---


신형 맥북 프로를 사서 이전에 쓰던 맥북프로에서 데이터를 옮기려고 했는데, 디스크 타입이 달라서 되지 않는다는 걸 확인하고처음으로 맥북에 `File System Personality`라는 것이 있다는 것을 알게 되었다.  

자신의 맥북에서 사용하는 포멧을 확인하려면 터미널에서 `diskutil info` 명령어를 입력하면 된다. 이중 `File System Personality` 부분을 확인하면 된다.

```bash
 $ diskutil info /
   Device Identifier:        disk1
   Device Node:              /dev/disk1
   Whole:                    Yes
   Part of Whole:            disk1
   Device / Media Name:      APPLE SSD AP0256J

   Volume Name:              Macintosh HD
   Mounted:                  Yes
   Mount Point:              /

   Content (IOContent):      Apple_HFS
   File System Personality:  Journaled HFS+
   Type (Bundle):            hfs
   Name (User Visible):      Mac OS Extended (Journaled)
   Journal:                  Journal size 24576 KB at offset 0x19502000
   Owners:                   Enabled

   OS Can Be Installed:      Yes
   Recovery Disk:            disk0s3
   Media Type:               Generic
   Protocol:                 PCI-Express
   SMART Status:             Not Supported
   Volume UUID:              
   Disk / Partition UUID:   

   Disk Size:                249.7 GB (249678528512 Bytes) (exactly 487653376 512-Byte-Units)
   Device Block Size:        4096 Bytes

   Volume Total Space:       249.7 GB (249678528512 Bytes) (exactly 487653376 512-Byte-Units)
   Volume Used Space:        196.4 GB (196400111616 Bytes) (exactly 383593968 512-Byte-Units) (78.7%)
   Volume Available Space:   53.3 GB (53278416896 Bytes) (exactly 104059408 512-Byte-Units) (21.3%)
   Allocation Block Size:    4096 Bytes

   Read-Only Media:          No
   Read-Only Volume:         No

   Device Location:          Internal
   Removable Media:          Fixed

   Solid State:              Yes
   Virtual:                  Yes
   OS 9 Drivers:             No
   Low Level Format:         Not supported

   This disk is a Core Storage Logical Volume (LV).  Core Storage Information:
   LV UUID:                
   LVF UUID:              
   LVG UUID:             
   PV UUID (disk):      
   Fusion Drive:             No
   Encrypted:                Yes

# 해당 결과만 얻고 싶으면 grep 명령어와 같이 사용해준다.
$ diskutil info / | grep File
```



> 굳이 터미널을 사용하지 않아도 Finder에서 `Command - i`를 눌러서 다음 정보로 확인 가능하다.

![finder-image](https://i.imgur.com/7bYI7zd.png)
