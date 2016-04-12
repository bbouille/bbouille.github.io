---
layout: post
title: "Truecrypt on Yosemite"
excerpt: "A post to fix truecrypt 7.1a installation on Yosemite"
tags: [osx, truecrypt]
---

## Environment

I got the idea to install TrueCrypt on my OSx running Yosemite (10.10.1) and found this sneaky welcome :
<br />
<br />
![TrueCrypt Error](/images/tc-71a-error.png)
<br />
<br />
The end of support from the [core development team](http://truecrypt.sourceforge.net) doesn't mean to stop using Truecrypt for me (see [here](https://www.grc.com/misc/truecrypt/truecrypt.htm) and [here](http://istruecryptauditedyet.com/) for the security audit results), I still need it to access my personal volumes. 
<br />
<br />
For now, the current release remains the 7.1a avalaible from [TCnext](https://truecrypt.ch/downloads/). It requires few tricks to get installed on Yosemite using the DMG file. This post introduces a method to bypass this error thanks to this [source](http://tips.tinyiron.net/yosemite-to-truecrypt-never-gonna-get-it/).
<br />
<br />
<span style="color:red">WARNING</span> please see the fork named `veracrypt` maintained by IDRIX to be up to date : [https://veracrypt.codeplex.com/](https://veracrypt.codeplex.com/).

## Method using the terminal/vim

1. Open the .dmg and extract the .mpkg in your home
2. Open the `~/TrueCrypt 7.1a.mpkg/Contents/distribution.dist` file with your favorite text editor
3. On line 13, replace this line :

```bash
if(!(system.version.ProductVersion >= '10.4.0')) {
```
  
by this one :

```bash
if(!(system.version.ProductVersion >= '10.10')) {
```
