---
title: NTFS read/write on OSx mountain lion
layout: post
permalink: /2013/08/04/ntfs-read-write-on-mountain-lion/
tags: [osx, ntfs]
---
How to read AND write NTFS hard drive on OSx 10.8.2 :

  1. Download and install MacFUSE: <a title="https://www.tuxera.com/mac/macfuse/MacFUSE-Tuxera-2.2.dmg" href="http://goo.gl/VdnPW" target="_blank">http://goo.gl/VdnPW</a>
  2. Download and install NTFS-3G: <a title="http://downloads.sourceforge.net/project/catacombae/NTFS-3G%20for%20Mac%20OS%20X/2010.10.2/ntfs-3g-2010.10.2-macosx.dmg?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fcatacombae%2Ffiles%2FNTFS-3G%2520for%2520Mac%2520OS%2520X%2F2010.10.2%2Fntfs-3g-2010.10.2-macosx.dmg%2Fdownload%3Fuse_mirror%3Diweb&ts=1345263864&use_mirror=iweb" href="http://goo.gl/HH0Pm" target="_blank">http://goo.gl/HH0Pm</a>
  3. Download and install the NTFS-3G timeout errors fix: <a title="http://cloud.github.com/downloads/bfleischer/fuse_wait/fuse_wait.pkg" href="http://goo.gl/nH5Or" target="_blank">http://goo.gl/nH5Or</a>
  4. Download and install OSXFUSE (be sure to check the Emulate MacFUSE checkbox): <a title="https://github.com/downloads/osxfuse/osxfuse/OSXFUSE-2.5.4.dmg" href="http://goo.gl/AP8kL" target="_blank">http://goo.gl/AP8kL</a>
  5. Open Disk Utility
  6. Right click on the appropriate drive (BOOTCAMP), select Mount

Thanks to [Sam Alston][1].

 [1]: http://kennelbound.com/write-support-for-ntfs-with-mac-osx-mountain-lion/