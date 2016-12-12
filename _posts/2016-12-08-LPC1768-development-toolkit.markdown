---
layout: post
title:  "LPC1768 development toolkit"
author: "WillyKaze"
date:   2016-12-08 12:16:25 +0100
thumbnail: microchip
categories: Tutorial
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

Install Git. Don't forget to check `▢ Add git to your PATH`  
You may have to logout/reboot to update the PATH variable.

Install your ARM tool chain. Note the install path.

## mbed-cli

With PyPi, this is simple. Open a shell (cmd.exe or PowerShell) and type:

```sh
$ pip install mbed-cli
```

# Configuration

First thing to do is to configure mbed to use your tool chain:

```sh
$ mbed config -G GCC_ARM_PATH "C:\Program Files (x86)\GNU Tools ARM Embedded\5.4 2016q3\bin"
```

This will set your GCC_ARM_PATH globally so you don't have to do it again when you change between project.
Of course, you can set a specific tool chain for a project by using

```sh
$ mbed config GCC_ARM_PATH "..\TOOLCHAIN_PATH\bin"
```

# Documentation

Here are some useful links to use mbed-cli and mbed-os:

[Mbed-CLI handbook][mbed-cli-handbook]

[Mbed-OS API reference][mbed-os-api-reference]

# Basic usage

## Initializing

In order to use mbed-cli, you need to instanciate a git repository. mbed-cli can do it for you:

```shell
$ mbed new [program_name]
```

If you already have a program folder:

```shell
$ cd PROGRAM_PATH
$ mbed new .
```

These command will initialize a git repository (if necessary), create some configuration files and clone a copy of mbed-os repository.
You can choose which version you want by going in the mbed-os directory and checkout the desirated branch or tag:

```shell
$ cd mbed-os\
$ git checkout master        # Development branch
$ git checkout mbed-os-5.2   # Active release branch
$ git checkout mbed-os-5.1   # Old release branch
$ git checkout mbed-os-5.2.3 # Specific tag
$ cd ..\
```

You can also use git submodules to handle your libraries.
For more information see the excellent [book about Git](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

## Compiling

If this is a new repo, you may have to specify the target and the tool chain:
```sh
$ mbed target LPC1768
$ mbed toolchain GCC_ARM
```

Once your repo is setup, you can start coding. Here is a quick example to check everything is working:

```cpp
/* Copyright 2016 Benoit Rapidel

   Licensed under the MIT License (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

        MIT

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
*/

/* Blink LED without using the mbed library. */

#include "mbed.h"

DigitalOut led1(LED1);
DigitalOut led2(LED2);
DigitalOut led3(LED3);
DigitalOut led4(LED4);

int main() 
{
    int i;

    while(1)
    {
        if (i % 1 == 0)
            led1 = !led1;

        if (i % 2 == 0)
            led2 = !led2;

        if (i % 4 == 0)
            led3 = !led3;

        if (i % 8 == 0)
            led4 = !led4;
        
        i = i + 1;
        Thread::wait(50);
    }
    return 0;
}
```

Save it to main.cpp and compile it:

```bash
$ mbed compile
$ mbed compile --color    # For color output
$ mbed compile -v         # Verbose output
$ mbed compile --config   # Display config and exit
```

If everything goes right, those commands will generate a .bin file under `BUILD/LPC1768/GCC_ARM/`.
An ELF file is also present to use with your debugger.

If you have and mbed LPC1768 platform, you can specify `--disk F:\` to upload the bin file directly onto the target.


[mbed-os]: https://github.com/ARMmbed/mbed-os
[mbed-cli]: https://github.com/ARMmbed/mbed-cli
[mbed-os-api-reference]: https://docs.mbed.com/docs/mbed-os-api-reference/en/5.2/
[mbed-cli-handbook]: https://docs.mbed.com/docs/mbed-os-handbook/en/5.2/dev_tools/cli/
