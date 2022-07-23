---
title: "Squirrel writeup"
date: 2022-03-18T02:01:38+06:00
description: Digging with eagle eye 
theme: Toha
summary: Hunt in a low level and keep your eagle eyes 🦅

menu:
  sidebar:
    name: Squirrel
    identifier: Squirrel
    parent: Write ups
    weight: 15
hero: "hero.jpg"
tags: ["Markdown","Content Organization","Multi-lingual"]
categories: ["Basic"]
---

# Zain CTF 2022 — Cybertalents


Category: Digital Forensics


Description : a picture tells a story. search for more details 


<!DOCTYPE html>
<html>
<head>
  <style>
.button-dl {
  border:  2px solid #008CBA;/* Blue */
  border-radius: 9px;
  color: black;
  padding: 9px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 15px;
  margin-left: 80px;
  cursor: pointer;
  background-color: #ffffff;
  transition-duration: 0.4s;
  box-shadow: 0 4px 10px 0 rgba(0,0,0,0.2), 0 4px 15px 0 rgba(0,0,0,0.19);
  }
.button-dl:hover {
  background-color: #008CBA; /* Blue */
  color: white;
}
.button-dl:active {
  background-color: #3e8e41;/* Green */
  box-shadow: 0 5px #666;
  transform: translateY(4px);

</style>
</head>
<body>


<a download =Squirrel  target = "_blank" class = "button-dl" href="/files/squirrel.zip" >Squirrel.zip</a>

The download link is a zipped file containing the squirrel image above. Once unzipped, I checked the file out.

```
└─$ file squirrel.jpg
squirrel.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 180x180, segment length 16
```
Looks normal.
let's check it's meta data of the file 
```
└─$ exiftool squirrel.jpg
ExifTool Version Number         : 12.40
File Name                       : squirrel.jpg
Directory                       : .
File Size                       : 412 KiB
File Modification Date/Time     : 2022:03:18 16:57:18-04:00
File Access Date/Time           : 2022:03:18 17:24:48-04:00
File Inode Change Date/Time     : 2022:03:18 17:24:31-04:00
File Permissions                : -rwxrw-rw-
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 180
Y Resolution                    : 180
Image Width                     : 640
Image Height                    : 480
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 640x480
Megapixels                      : 0.307

```
Looks normal too.. ok


Let's check strings and it produces a ton of output.

I find it’s easier to find any maybe .jpg /.zip or also any long strings maybe helpful

for print any string include ".jpg"/".zip" we use grep 
```js
$ strings squirrel.jpg  | grep '.jpg'
 ```
for print any sequence of at least 15 
```js
$ strings -n 15 squirrel.jpg

```
Nothing with .jpg but BOOM.
either with ".zip" or long strings 

```
└─$ strings -n 15 squirrel.jpg                           
ioncfvl/kdal/je
afgnbvikmjkwvasbf7dw es 6qtntekias dteewf.zas/ddsas.......
https://www.mediafire.com/file/yuy6pf4pj3004em/iamnotreal.zip/file
cellTextIsHTMLbool
ESliceHorzAlign
ESliceVertAlign
bgColorTypeenum
ESliceBGColorType
bottomOutsetlong
rightOutsetlong
Canon PowerShot S2 IS
Adobe Photoshop Elements 4.0 Windows
2007:01:13 19:09:01
2007:01:10 11:58:45
2007:01:10 11:58:45
Copyright (c) 1998 Hewlett-Packard Company
sRGB IEC61966-2.1
sRGB IEC61966-2.1
IEC http://www.iec.ch
IEC http://www.iec.ch
.IEC 61966-2.1 Default RGB colour space - sRGB
.IEC 61966-2.1 Default RGB colour space - sRGB
,Reference Viewing Condition in IEC61966-2.1
,Reference Viewing Condition in IEC61966-2.1
http://ns.adobe.com/xap/1.0/

```
There is a link from media fire having "iamnotreal.zip" file , let's check it

The zip file is protected by a password let's try crack it using john

```
└─$  zip2john iamnotreal.zip > iamnotreal.john
Created directory: /home/kali/.john
ver 2.0 efh 5455 efh 7875 iamnotreal.zip/evil/EVIL PKZIP Encr: TS_chk, cmplen=415382, decmplen=422982, crc=0B9A27A8 ts=982F cs=982f type=8

```
   "-\- wordlist = " path may varies depend on its location

```
└─$ john iamnotreal.john --wordlist=/usr/share/wordlists/rockyou.txt 

Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
squirrel07       (iamnotreal.zip/evil/EVIL)     
1g 0:00:00:00 DONE (2022-03-21 11:48) 4.166g/s 5290Kp/s 5290Kc/s 5290KC/s srsrdm..solrampiche
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```
then unzip it


```js
└─$ unzip iamnotreal.zip
  Archive:  iamnotreal.zip
  [iamnotreal.zip] evil/EVIL password: 
    inflating: evil/EVIL
```
```js
└─$ cd evil 

└─$ file EVIL
EVIL: data
```
It's unknown data file , let's keep analyse it.

let's check hex values by `xxd` , mostly already installed in linux 

if not use  `$ sudo apt install xxd`

```
└─$ xxd EVIL | more
00000000: a119 74bc 0010 4a46 4946 0001 0101 0048  ..t...JFIF.....H
00000010: 0048 0000 ffed 1b62 5068 6f74 6f73 686f  .H.....bPhotosho
00000020: 7020 332e 3000 3842 494d 0425 0000 0000  p 3.0.8BIM.%....
00000030: 0010 0000 0000 0000 0000 0000 0000 0000  ................
00000040: 0000 3842 494d 03ed 0000 0000 0010 0048  ..8BIM.........H
00000050: 0000 0001 0002 0048 0000 0001 0002 3842  .......H......8B
00000060: 494d 0426 0000 0000 000e 0000 0000 0000  IM.&............
00000070: 0000 0000 3f80 0000 3842 494d 040d 0000  ....?...8BIM....
00000080: 0000 0004 0000 001e 3842 494d 0419 0000  ........8BIM....
00000090: 0000 0004 0000 001e 3842 494d 03f3 0000  ........8BIM....
000000a0: 0000 0009 0000 0000 0000 0000 0100 3842  ..............8B
000000b0: 494d 040a 0000 0000 0001 0000 3842 494d  IM..........8BIM
000000c0: 2710 0000 0000 000a 0001 0000 0000 0000  '...............
000000d0: 0002 3842 494d 03f5 0000 0000 0048 002f  ..8BIM.......H./
000000e0: 6666 0001 006c 6666 0006 0000 0000 0001  ff...lff........
000000f0: 002f 6666 0001 00a1 999a 0006 0000 0000  ./ff............
00000100: 0001 0032 0000 0001 005a 0000 0006 0000  ...2.....Z......
00000110: 0000 0001 0035 0000 0001 002d 0000 0006  .....5.....-....
00000120: 0000 0000 0001 3842 494d 03f8 0000 0000  ......8BIM......
00000130: 0070 0000 ffff ffff ffff ffff ffff ffff  .p..............
00000140: ffff ffff ffff ffff ffff 03e8 0000 0000  ................
00000150: ffff ffff ffff ffff ffff ffff ffff ffff  ................
00000160: ffff ffff ffff 03e8 0000 0000 ffff ffff  ................
00000170: ffff ffff ffff ffff ffff ffff ffff ffff  ................
00000180: ffff 03e8 0000 0000 ffff ffff ffff ffff  ................
00000190: ffff ffff ffff ffff ffff ffff ffff 03e8  ................
000001a0: 0000 3842 494d 0408 0000 0000 0010 0000  ..8BIM..........
000001b0: 0001 0000 0240 0000 0240 0000 0000 3842  .....@...@....8B
000001c0: 494d 041e 0000 0000 0004 0000 0000 3842  IM............8B
000001d0: 494d 041a 0000 0000 034b 0000 0006 0000  IM.......K......
000001e0: 0000 0000 0069 0000 0761 7364 0a20 0000  .....i...asd. ..
000001f0: 696f 6e63 6676 6c2f 6b64 616c 2f6a 65bc  ioncfvl/kdal/je.
00000200: 6166 676e 6276 696b 6d6a 6b77 7661 7362  afgnbvikmjkwvasb
00000210: 6637 6477 2065 7320 3671 746e 7465 6b69  f7dw es 6qtnteki
00000220: 6173 2064 7465 6577 662e 7a61 732f 6464  as dteewf.zas/dd
00000230: 7361 732e 2e2e 2e2e 2e2e 0000 0000 0000  sas.............
00000240: 0000 0000 0000 0000 0001 0000 0000 0000  ................
00000250: 0000 0000 0a20 0000 0798 0000 0000 0000  ..... ..........
00000260: 0000 0000 0000 0000 0000 0100 0000 0000  ................
00000270: 0000 0000 0000 0000 0000 0000 0000 1000  ................
00000280: 0000 0100 0000 0000 6874 7470 733a 2f2f  ........https://
00000290: 7777 772e 6d65 6469 6166 6972 652e 636f  www.mediafire.co
000002a0: 6d2f 6669 6c65 2f6d 7035 386a 6775 6770  m/file/mp58jgugp
000002b0: 7361 6839 3667 2f69 616d 6e6f 7472 6561  sah96g/iamnotrea
000002c0: 6c2e 7a69 702f 6669 6c65 006e 756c 6c00  l.zip/file.null.
000002d0: 0000 0200 0000 0662 6f75 6e64 734f 626a  .......boundsObj
000002e0: 6300 0000 0100 0000 0000 0052 6374 3100  c..........Rct1.
000002f0: 0000 0400 0000 0054 6f70 206c 6f6e 6700  .......Top long.
00000300: 0000 0000 0000 004c 6566 746c 6f6e 6700  .......Leftlong.
00000310: 0000 0000 0000 0042 746f 6d6c 6f6e 6700  .......Btomlong.
00000320: 0007 9800 0000 0052 6768 746c 6f6e 6700  .......Rghtlong.
00000330: 000a 2000 0000 0673 6c69 6365 7356 6c4c  .. ....slicesVlL
00000340: 7300 0000 014f 626a 6300 0000 0100 0000  s....Objc.......
00000350: 0000 0573 6c69 6365 0000 0012 0000 0007  ...slice........
00000360: 736c 6963 6549 446c 6f6e 6700 0000 0000  sliceIDlong.....
00000370: 0000 0767 726f 7570 4944 6c6f 6e67 0000  ...groupIDlong..
00000380: 0000 0000 0006 6f72 6967 696e 656e 756d  ......originenum
--More--

```

it's a bit complex but at the first look , there's "JFIF" seems like a file extension format.
after searching , it's format for pictures but with minmal amount of data like header , let's check the original picture hexa just the first head line to save time 
```
└─$ xxd squirrel.jpg | head -n1                                                    
00000000: ffd8 ffe0 0010 4a46 4946 0001 0101 00b4  ......JFIF......

```

sounds look alike but with a little change

but is missing the file signature (in the first four bytes)

then let's try to change this header by any hex editor 
in my case i used Ghex 

`
sudo apt install ghex
`

and write the same header for the ".jpg" file instead the current header and now check the file it's a picture now.
<img class = "img" src="/files/EVIL.jpeg" alt="evil squirrel">

bring the writen on the top right side to <a href="https://gchq.github.io/CyberChef/">Cypher Chef</a>
<br>
And it's encoded base32 string and BOOM
The flag : `flag{Ev1l_S9uirr3lz}`