I got the idea to install TrueCrypt on my OSx running Yosemite (10.10.1) and found this sneaky welcome :

![TrueCrypt Error](/assets/tc-71a-error.png)

With the end of support from the [core development team](http://truecrypt.sourceforge.net) and the ongoing reborn of Truecrypt (see [here](https://www.grc.com/misc/truecrypt/truecrypt.htm) for background and [here](http://istruecryptauditedyet.com/) for audit results), I have some hopes to use Truecrypt normally very soon.
 
For now, the current release remains the 7.1a avalaible from [TCnext](https://truecrypt.ch/downloads/). It requires few tricks to get installed on Yosemite using the DMG file. This post compiles two different methods to bypass this error.

###Method 1 : using Finder

1. Open the .dmg (check your [security settings](http://technofyi.com/2013/11/18/open-untrusted-apps-os-x/) if the 'unidentified developer' pops up)

2. You’ll find the .mpkg file, right-click and “Show Package Contents”

3. Open 'Contents Dir'

4. Open 'Packages Dir'

5. Install each of the 4 packages in this order: 
  1. OSXFUSECore.pkg
  2. OSXFUSEMacFUSE.pkg
  3. MacFUSE.pkg
  4. TrueCrypt.pkg (It is possible MacFUSE.pkg will install the two before it, but we ran each to play it safe.).

[Source](http://tips.tinyiron.net/yosemite-to-truecrypt-never-gonna-get-it/)

###Method 2 : using the terminal/vim

1. Open the .dmg and extract the .mpkg in your home
2. Open the '~/TrueCrypt 7.1a.mpkg/Contents/distribution.dist' file
3. On line 13, replace this line :
     * if(!(system.version.ProductVersion >= '10.4.0')) {
  
  by this one :
     * if(!(system.version.ProductVersion >= '10.10')) { 