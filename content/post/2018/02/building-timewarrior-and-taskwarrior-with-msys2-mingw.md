---
title: "Building Timewarrior and Taskwarrior with msys2/mingw"
date: 2018-02-22T10:20:03+01:00
lastmod: 2018-02-26T14:22:00+01:00
categories: ["Blog"]
tags: ["mingw", "tools"]
---

This post describes how to build [taskwarrior][taskwarrior] and [timewarrior][timewarrior] from github
sources on a Windows system using msys & mingw.

**2018-02-26 Update**:  added information about run-time dependencies
[taskwarrior]: https://github.com/GothenburgBitFactory/taskwarrior
[timewarrior]: https://github.com/GothenburgBitFactory/timewarrior
<!--more-->

## Install Environment
* install [msys2][msys2] according to [this installation procedure][msys2]. I used the `x86_64` 64-bit 
  version (e.g. `msys2-x86_64-20161025.exe`)
* execute

```
msys2-x86_64-20161025.exe
```

*  open the shell afterwards and do a "system" update with

```
pacman -Syu
```

accept all "replacement" suggestions, after the update kill shell

and then once more open new shell  execute agin

```
pacman -Syu
```

* after the successful intstallion and update procedure described above open msys2 shell: all following 
  commands are to be executed from within this shell
* install build dependencies `msys2/gcc` and associated dev tools 

```
pacman -S msys2-devel make cmake git
```

* after that i installed the `{task,time}warrior` specific things 

```
pacman -S libgnutls-devel  python  libutil-linux-devel
```

During run-time the following package is needed

```
pacman -S libhogweed
```

if the `libhogweed` is not installed you will get the following (not very helpful) error message: 

```
error while loading shared libraries: ?: cannot open shared object file: No such file or directory
```

I did not need to install any `mingw32/mingw-w64-i686-gcc` (coming with the `mingw-w64-i686-toolchain` bundle) 
or `mingw64/mingw-w64-x86_64-gcc` (coming with the `mingw-w64-x86_64-toolchain` bundle) compilers. on the contrary, 
I was quite unsuccesfull trying to do so ;-).

## Building Taskwarrior

```
cd /usr/src
git clone --recursive -b 2.6.0 https://github.com/GothenburgBitFactory/taskwarrior.git
cd taskwarrior
cmake -DCMAKE_SYSTEM_NAME=CYGWIN-GNU  -DCMAKE_BUILD_TYPE=release .
make
make install
```

## Building Timewarrior

```
cd /usr/src
git clone --recursive -b 1.1.1 https://github.com/GothenburgBitFactory/timewarrior.git
cd timewarrior
cmake -DCMAKE_SYSTEM_NAME=CYGWIN-GNU  -DCMAKE_BUILD_TYPE=release .
make
make install
```

if you get the following error message during the clone command:

```
fatal: clone of 'https://git.tasktools.org/TM/libshared.git' into submodule path '/usr/src/timewarrior/src/libshared' failed

```

you need to change the reference to the `libshared` git repository within the `taskwarrior/.gitmodules` and replace

 `https://git.tasktools.org/TM/libshared.git`

with

 `https://github.com/GothenburgBitFactory/libshared.git`

Afterwards do

```
cd timewarrior
git submodule sync
git submodule update --remote
```

and then continure with the  `cmake ...` command above.

## Personal Report support

to run my personal timewarrior reports the following steps have to be done in addition

```
pacman -S python3-setuptools
pip install PyYaml python-dateutil timew-report
```

[msys2]: http://www.msys2.org/