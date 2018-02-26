---
layout: post
title:  "ExMachina source code organization policies"
author: "WillyKaze"
date:   2018-02-26 12:12:00 +0100
thumbnail: code
categories: Tutorial
tags: Git submodules
---
This is a memo for source code organization policies applied at ExMachina.  
The main purposes for those policies is to guaranty a good code quality which
enables users to share, review and comment code.  
**Note:** This is primary requirement to make our open-source philosophy possible.
It also allows us to implement CI and CD easily.  
For any Git question, refer to the [excellent git book][git-book] before asking.

# Git repository structure
Let's begin with a basic repository structure for an mbed project.
```text
/
├── docs/
│   └── Doxyfile        // Configuration file for doxygen
├── lib/                // Contains all the library used inside this project
│   ├── libAC780        // LCD screen library pulled by a git-submodule
│   ├── libLocal        // Library only used in this project can be used without submodules
│   └── libQEI          // Encoder library pulled by a git-submodule
├── mbed-os/            // Mbed source code, always pulled as a git-submodule
├── src/
│   ├── subfolder/      // Folder use to sort your files
│   ├── main.cpp        // Main file for your project
│   └── main.h
├── scripts/            // Contains scripts use to build, make this project (Optionnal)
├── tools/              // Contains tools for this project (i.e. nxp-flasher)
│   └── nxp-flasher     // Flasher tool always pulled as a git-submodule
├── .gitlab-ci.yml      // Gitlab CI configuration file
├── .gitignore
├── .gitmodules         // Holds project git-submodules URL
├── .mbed               // Mbed specific configuration file
├── .mbedignore         // Mbed specific configuration file
├── LICENSE             // License file
├── make.bat            // Windows make script (build and flash)
├── mbed_settings.py    // Mbed specific configuration file
└── README.md           // General README file
```

Note that all folder names are **lowercase**.

# Git submodules

We talked a lot about git submodules in the previous chapter. Here we'll dive in
the most awesome feature of git (IMHO).

To clone a project with git submodules, we use the following command:
```
$ git clone --recurse-submodules git@git.host.tld:group/repo.git
```
The `--recurse-submodules` flag will also clone the submodules specified in the
`.gitmodules` file with the __exact__ commit specified.  
This way we are sure we pull a working copy of this repository even if the libraries
have been updated.

**Warning:** That's why when adding a submodule, the URL specified should always
be publicly accessible. So anyone can pull your code with the submodules.

If we forgot to add the `--recurse-submodules` flag while cloning the repository,
we can always initialize them afterwards with:

```sh
$ git submodule init
$ git submodule update
```

This will fetch the data into the directory specified in the `.gitmodules` file.

## Adding a submodule

TODO


*[CI]: Continuous integration
*[CD]: Continuous deployment
*[IMHO]: In My Humble Opinion
[git-book]: https://git-scm.com/book
[git-book-submodules]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
