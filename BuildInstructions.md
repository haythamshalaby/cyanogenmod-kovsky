# Introduction #

You can manually build CyanogenMod from CoolRunnerII's GitHub repository. Doing so gives you the benefit of newer fixes not yet in pre-built packages. These instructions are tested and specifically for Ubuntu 12.04 and later, but it may or may not work on other distributions with some altered and adapted commands.

# Preparation #

## Step 1: Set Up Platform Tools ##

**Ubuntu 12.10+:** Starting with Ubuntu 12.10+, ADB (Android Debug Bridge) and Fastboot are now included in the the official Ubuntu repositories. You can install both these packages using `apt-get`:
```
android-tools-adb android-tools-fastboot
```

**Universal:** Download the [Android SDK](http://developer.android.com/sdk/index.html). The ADT (Android Developer Tools) Bundle isn't necessary for the build process so downloading only the SDK (Software Development Kit) is enough. Follow the preparation and installation procedures from the [Ubuntu Wiki](https://help.ubuntu.com/community/AndroidSDK).

## Step 2: Install Packages ##

To install packages, use the command `sudo apt-get install [packages]`.

Install the following packages:
```
git-core gnupg flex bison gperf libsdl1.2-dev libesd0-dev libwxgtk2.8-dev squashfs-tools build-essential zip curl 
libncurses5-dev zlib1g-dev pngcrush schedtool
```

If you have a 64-bit OS, add the following packages:
```
g++-multilib lib32z1-dev lib32ncurses5-dev lib32readline-gplv2-dev gcc-4.7-multilib g++-4.5-multilib
```

You have an option to install OpenJDK 6 or Oracle's JDK 6. For Oracle's JDK 6, follow this tutorial from [Web Upd8](http://www.webupd8.org/2012/11/oracle-sun-java-6-installer-available.html). For OpenJDK, install the following packages:
```
openjdk-6-jdk openjdk-6-jre
```

## Step 3: Create Directories ##

Create these directories to setup your build environment:
```
$ mkdir -p ~/bin
$ mkdir -p ~/android/system
```

Include `~/bin` in your path using this: (Assuming you're using the BASH shell.)
```
$ export PATH=${PATH}:~/bin
```

**Hint:** You can edit the `.bashrc` file (located in the root of your home directory) to add the PATH variable. This makes the path you set for `~/bin` permanent. This can also be used for the Android SDK's platform tools.

## Step 4: Install `repo` Script ##

Download the `repo` script [here](https://dl-ssl.google.com/dl/googlesource/git-repo/repo) or via terminal:
```
$ curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > ~/bin/repo
```

Allow the `repo` script to be executable with this command:
```
$ chmod a+x ~/bin/repo
```

## Step 5: Initialize Repository ##

Download and initialize the repository:
```
$ cd ~/android/system
$ repo init -u git://github.com/CoolRunnerII/android.git -b gb-release-7.2
$ repo sync -j16
```

**Note:** This may take several hours, depending on your internet connection. (Approximately 8-15 hours with a 500 KB/s network.)

# Copy Proprietary Files #

**Note:** These steps require to have Android already installed on your device.

## Step 1: Enable USB Debugging ##

Make sure your device is rooted first. On your device, enable Android USB Debugging by going to `Settings > Applications > Development` and select `Android debugging`. Verify your device is recognized by using the following command:
```
$ adb devices
```

It should probably look something like this: (Numbers may vary.)
```
* daemon not running. starting it now on port 5037 *
* daemon started successfully *
List of devices attached 
000000000000	device
```

## Step 2: Copy the Proprietary Blobs ##

Extract your device's proprietary files using this script:
```
$ cd ~/android/system/device/htc/kovsky
$ ./extract-files.sh
```

Copy the vendor file to the correct directory:
```
$ cp ~/android/system/device/htc/kovsky/cyanogen_kovsky.mk ~/android/system/vendor/cyanogen/products
```

# Download ROM Manager #

Download ROM Manager by running the following script:
```
$ cd ~/android/system/vendor/cyanogen
$ ./get-rommanager
```

Sync the repository again to download any new commits: (If any.)
```
$ cd ~/android/system
$ repo sync
```

# Start Building #

Prepare build by executing the following commands:
```
$ . build/envsetup.sh 
$ lunch cyanogen_kovsky-eng
```

Start building:
```
$ make bacon
```

**Note:** This may take about 5-8 hours, depending on the speed of you computer.

If there is an error involving the incorrect Java version (requires 1.6.0, 1.7.0 will NOT work), execute the following commands:
```
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
```

Change it to the correct version number. (`java-6-openjdk` or `java-6-sun`)

# Installation #

You're newly built firmware is waiting for you at `~/android/system/out/target/product/kovsky`. Simply flash your freshly compiled build of CyanogenMod with ClockworkMod Recovery or ROM Manager. Have fun!

# Commiting changes #

If you have made changes to the files in the device folder and want to commit them you may contact CoolRunnerII on XDA-Developers to obtain write permissions. After, do the following.:

```
$ git remote add --track gb-release-7.2 github_push git@github.com:CoolRunnerII/device_kovsky.git
$ git commit -a
$ git push github_push gb-release-7.2
```