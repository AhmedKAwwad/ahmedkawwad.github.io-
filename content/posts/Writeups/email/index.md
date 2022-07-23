---
title: "Email Address writeup"
date: 2022-03-18T01:01:28+04:00
summary: Adding intel to the skills and use your sherlock sens🧐
menu:
  sidebar:
    name: Email Address
    identifier: Email Address
    parent: Write ups
    hero: "hero.jpg"
    weight: 15
---


### Zain CTF 2022 — Cybertalents


##### Description : Can find email from just username "c5oylchicrlnofu "

##### Category: Open Source Cyber Intelligence 

One of the most known tools to collect all links related to a username is <a href = "https://github.com/sherlock-project/sherlock" >Sherlock</a>.


Installation for linux:
```
# clone the repo
$ git clone https://github.com/sherlock-project/sherlock.git

# change the working directory to sherlock
$ cd sherlock

# install the requirements
$ python3 -m pip install -r requirements.txt
```
then we run it for "c5oylchicrlnofu" 
```
[*] Checking username c5oylchicrlnofu on:
[+] CapFriendly: "https://www.capfriendly.com/users/c5oylchicrlnofu"
[+] Chess: "https://www.chess.com/member/c5oylchicrlnofu"
[+] Coil: "https://coil.com/u/c5oylchicrlnofu"
[+] Fiverr: "https://www.fiverr.com/c5oylchicrlnofu"
[+] GitHub: "https://www.github.com/c5oylchicrlnofu"
[+] Gumroad: "https://www.gumroad.com/c5oylchicrlnofu"
[+] HackerNews: "https://news.ycombinator.com/user?id=c5oylchicrlnofu"
[+] Instagram: "https://www.instagram.com/c5oylchicrlnofu"
[+] LeetCode: "https://leetcode.com/c5oylchicrlnofu"
[+] Minecraft: "https://api.mojang.com/users/profiles/minecraft/c5oylchicrlnofu"
[+] Smule: "https://www.smule.com/c5oylchicrlnofu"
[+] Spotify: "https://open.spotify.com/user/c5oylchicrlnofu"
[+] TradingView: "https://www.tradingview.com/u/c5oylchicrlnofu/"
[+] Venmo: "https://venmo.com/u/c5oylchicrlnofu"
```

<!DOCTYPE html>
<html>
<head>
</head>
<body>
<br>
so let's dig in at all these links and see if there's any 
accounts with public email or we can get its email. 

GitHub Account seems sus for me  as it's recently created and also has only 1 repo called "
MyFirstRepository".

Checking this repo , i find it has 2 commits
<br><br>
<img class = "img" src="/files/repo.png" alt="repo-screen" width="900" height="230" >
<br><br>
A bit of more digging you can find that for commits there is a details can get more info by clickin on commit ID .

<br><br>
<img class = "img" src="/files/commit.png" alt="commits-screen" width="900" height="230" >
<br><br>

By clicking on it , changes done in this commit appears and also URL now including "commit ID" 
```
"https://github.com/c5oylchicrlnofu/MyFirstRepository/commit/9f15379fa7f92d37c6a5dda62ee9df73367709db"
```

### And here is the trick :
you can always git more details about the author's email of commit  by adding ".patch" after the URL of commit details as following:

```
"https://github.com/c5oylchicrlnofu/MyFirstRepository/commit/9f15379fa7f92d37c6a5dda62ee9df73367709db.patch"

```
And BOOM here we go:

```
From 9f15379fa7f92d37c6a5dda62ee9df73367709db Mon Sep 17 00:00:00 2001
From: c5oylchicrlnofu <ch05o6wo7ribrlfrldrusp1ke@protonmail.com>
Date: Tue, 8 Mar 2022 19:54:26 +0300
Subject: [PATCH] Update README.md

---
 README.md | 4 +++-
 1 file changed, 3 insertions(+), 1 deletion(-)

diff --git a/README.md b/README.md
index 52b6d5a..a479a79 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,3 @@
-# MyFirstRepository
\ No newline at end of file
+# MyFirstRepository
+
+This is my first repository
```

His email : <ch05o6wo7ribrlfrldrusp1ke@protonmail.com>



Then since flag format  is : flag{email}

`flag{ch05o6wo7ribrlfrldrusp1ke@protonmail.com}`

</body>
</html>