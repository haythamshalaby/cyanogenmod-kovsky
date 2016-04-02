# Introduction #

If you're having trouble with the SD card not getting recognized, booting with it on, not being able to write on it, etc., maybe this fix will solve your problem. Credits to BrickPSP for this fix and Elibl for the original post.

# iBored #

iBored is a free cross-platform disk sector hex editor. You're going to need this to modify the disk sectors. Don't worry, you won't have to do it all manually. Download the application here for your specific operating system:

  * **Homepage:** http://apps.tempel.org/iBored/
  * **Windows (Vista+):** http://files.tempel.org/iBored/iBored-Windows_1.1.19.zip
  * **Windows (XP):** http://files.tempel.org/iBored/iBored-Windows_1.1.19-XP.zip
  * **Macintosh (Intel):** http://files.tempel.org/iBored/iBored-Mac_1.1.19.zip
  * **Macintosh (PowerPC):** http://files.tempel.org/iBored/iBored-MacPPC_1.1.5.zip
  * **Linux:** http://files.tempel.org/iBored/iBored-Linux_1.1.19.zip
  * **Other Versions:** http://files.tempel.org/iBored/

# iBored Method #

Download this file [here](http://code.google.com/p/cyanogenmod-kovsky/downloads/detail?name=sdsectorfix.dat&can=2&q=). Move the file to wherever you want, but the iBored directory is recommended. Now follow these steps:

  1. Launch iBored as an administrator. (Root for Macintosh and Linux)
  1. Identify your SD card. (**WARNING:** physicaldrive0 is your hard drive! Do **NOT** write on that hard drive. This will cause serious problems with your operating system.)
  1. Select your SD card and go to Disk > View Structure.
  1. Select your SD card's disk# and go to Disk > Write File to Blocks...
  1. Browse and select the file sdsectorfix.dat. Confirm everything, do **NOT** modify any other values.
  1. Format your SD card to FAT32 with the default allocation size.
  1. You're done! Have fun with your device with a recognized SD card!

# Alternate Method (Linux Only) #

Here is an alternate fix for Linux systems. Credits to zargloub.

  1. Launch the Disk Utilities application
  1. Identify your SD card partition (/dev/sd**,** being the drive "letter". e.g. /dev/sdb, /dev/sde, etc.)
  1. Run a quick format (FAT32)
  1. Launch the Terminal and enter `mkdosfs -I -S 512 /dev/sd*`.
  1. Verify with the command `dmesg | grep sd.*logical`. It should display something like this: (Results may vary)
```
[25586.667375] sd 4:0:0:0: >[sdb] 15564800 512-byte logical blocks: (7.96 GB/7.42 GiB)
```

# Notes #

On several occasions, your device will display you SD card is damaged. If you can read and write files in your SD card properly, do **NOT** reformat it on your device's firmware. Simply reboot and it will work properly. If problem persists, backup your files and repeat the fix again.

Also, you may have issues reformatting and partitioning on Linux with the iBored method. If so, use the alternate method.