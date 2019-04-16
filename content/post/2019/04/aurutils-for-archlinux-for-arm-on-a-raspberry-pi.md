---
title: "aurutils for Archlinux for ARM on a Raspberry PI"
date: 2019-04-07T21:44:41+02:00
categories: ["Blog"]
tags: []
draft: true
---


`setarch` is not possible on ARM, needs to be disabled in `/usr/bin/arch-nspawn` 

```
unset CARCH 
```

or similar

also missing `/usr/share/devtools/makepkg-armv7l.conf`

can this be derived from `makepkg-x86_64.conf`?
