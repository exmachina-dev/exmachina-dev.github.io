---
layout: post
title:  "LPC1768 development toolkit"
author: "WillyKaze"
date:   2016-12-08 12:16:25 +0100
tags: LPC1768 mbed
---
This is a quick tutorial to install the basic toolkit required for compiling and debugging the LPC1768 using [mbed-os][mbed-os].

# Requirements

* [Python 2.7](https://www.python.org/downloads/)  
  Be sure to select Python 2.7 release, mbed-cli is not compatibly with Python 3
* [Git](https://git-scm.com/downloads/)  
  On Windows, add git to your `PATH`
* A ARM Toolchain – we use [GNU ARM Embedded Toolchain](https://launchpad.net/gcc-arm-embedded)
* Mercurial is optional, only install if you use hg repositories.


# Installation

## Installing requirements
Start by installing Python 2.7 with PyPi.

Install Git. Don't forget to check `Add git to your PATH`
You may have to logout/reboot to update the PATH variable.

Install your ARM tool chain. Note the install path.

## mbed-cli

With PyPi, this is simple. Open a shell (cmd.exe or PowerShell) and type:

```sh
pip install mbed-cli
```

# Configuration

First thing to do is to configure mbed to use your tool chain:

```sh
mbed config -G GCC_ARM_PATH="C:\Program Files (x86)\GNU Tools ARM Embedded\5.4 2016q3"
```

To check everything is okay, go to a program repository and do

```sh
mbed compile
```

If you have an mbed platform, you can specify the target device to directly copy the firmware like so:

```sh
mbed compile --disk F:\
```


[mbed-os]: https://github.com/ARMmbed/mbed-os
[mbed-cli]: https://github.com/ARMmbed/mbed-cli
[mbed-os-api-reference]: https://docs.mbed.com/docs/mbed-os-api-reference/en/5.2/
[mbed-cli-handbook]: https://docs.mbed.com/docs/mbed-os-handbook/en/5.2/dev_tools/cli/
