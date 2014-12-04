---
layout: post
title: Progress on Stage7
description: In this entry I just a brief update on Stage7 installer
date: 2014-12-04 12:13:25 -08:00
tags: "Digital Survival, Stage7, Gentoo Installer, POSIX"
---

This week I've been able to make some decent progress on the Stage7 installer. As of version 0.1.1-alpha there is now a check to determine if essential S7 modules are missing. It doesn't actually extend the functionality of the code, but it is a step in the right directly as far as the stability of the installer. Next I will make sure the live medium contains the required tools. Most of them are generic GNU core utilities (echo, chmod, sort, etc.), but a few "outside" tools to exist. Those are the important ones to check for. These things will not matter much as long as people use Stage7 installer from a Digital Survival LiveCD/USB because I will make sure all the utilities are available, but if other Gentoo-based distributions want to use Stage7 it will be helpful for them to determine they are missing something.

I hope to push a few commits this weekend, and then focus more intensely on actually extending functionality to include Grub 2 and GPT support. God willing I'll be able to get those kinds of issues worked out rather quickly.

Onward and upward!
Maffblaster