---
title: "Equat1on writeup"
date: 2022-03-18T01:01:28+04:00
description: Zain CTF 2022 — Cybertalents
summary: Coding mixed with Quick math 😉
menu:
  sidebar:
    name: Equat1on
    identifier: Equat1on
    parent: Write ups
    weight: 15
---

### Zain CTF 2022 — Cybertalents

Description : You won't pass our gate!

Category: Crytpography

here is the challenge file , is a python code
as it seems open a file “flag.txt” and read it


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

<a download =challenge.py  target = "_blank" class = "button-dl" href="/files/challenge.py" >Challenge.py</a>

```
#!python3 
from math import *
with open('flag.txt','r') as fg:
  flag=fg.read()
```
### calc(x,val) function:

```
  def calc(x,val):
    return {
      0: lambda x: (x+1)/(x-1),
      1: lambda x: 984512/(x-69964)+69964
    }[val](x)
```
seems it check if “val” is “0” > x = (x+1)/(x-1)
if “val” is “1”> x = 984512/(x-69964)+69964
x is the ascii order passed to get_val(x,i)

### Main function:

```
  enc=[]
  i= 0
  for L in flag:
    enc.append(get_val(ord(L),i))
    i+=1


```
read every letter in flag and read it with ord() which gets the Ascii order of the letter.

Then pass it to get_val() function and append it to enc variable “enc.append()”
### get_val (x,i) function:

```
  def get_val(x,i):
    if ( i % 2 == 0 ):
      return calc(x,1)
    else:
      return calc(x,0)
```
seems it check if “i” is even > pass the x parameter and “1” to calc() function
if “i” is odd > pass the x parameter and “0” to calc() function
and there’s a comment down there

```
[69949.90776101458, 1.0186915887850467, 69949.90876951923, 1.0196078431372548, 69949.90352371817, 1.0277777777777777, 69949.90614710683, 1.017094017094017, 69949.90594534237, 1.0186915887850467, 69949.90473463427, 1.017391304347826, 69949.90715584248, 1.018181818181818, 69949.90614710683, 1.024390243902439, 69949.90917288068, 1.0208333333333333, 69949.90534001432, 1.04, 69949.90917288068, 1.02, 69949.90876951923, 1.0175438596491229, 69949.90392737999, 1.0212765957446808, 69949.90574357212, 1.02, 69949.90876951923, 1.0175438596491229, 69949.90392737999, 1.0212765957446808, 69981.0168870452, 1.0212765957446808, 69949.90513822675, 1.0178571428571428, 69949.90473463427, 1.0192307692307692, 69949.90372555197, 1.0166666666666666, 69949.90312003322, 1.2222222222222223]
```
seems like the flag results after the whole operation, then decided to reverse it

Then enc will be filled with “asci order number ” of each character in flag after performing the above equation depends on his order in flag string which is in flag.txt ,

either odd order {1,3,5,…etc} or even order {0,2,4,..etc}
so, i reversed each equation in a function depending on their order too.

Here is the Mathematics part to get the Ascii order out of the "enc" value we reverse equation when i = 1 (even order) 
<br><br>
<img class = "img" src="/files/equ-even.JPG" alt="equation for even" width="250" height="150" style="vertical-align:middle;margin:0px 450px">
<br><br>
Here is the Mathematics part to get the Ascii order out of the "enc" value we reverse equation when i = 1 (odd order)
<br><br>
<img class = "img" src="/files/equ-odd.JPG" alt="equation for odd" width="350" height="150" style="vertical-align:middle;margin:0px 400px">
<br><br>
Here is how we can scripit that equation depending on order of each enc and return back the ascii order and then read it as letter.

then append each letter reversed from the commented enc list to “flag”

<a download =reverseEquation.py target = "_blank" class = "button-dl" href="/files/reverseEquation.py" >reverseEquation.py</a>




Then the flag come up : `flag{InvolutionS_ar3_easy_peasy_🍋_squizy}`
</body>
</html>