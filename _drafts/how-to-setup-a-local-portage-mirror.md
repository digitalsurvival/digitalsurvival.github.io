---
layout: post
title: How to Setup a Local Portage Mirror
description: In this entry you can learn how to create a local Portage Mirror.
date: 2014-10-15 14:10:26 -07:00
tags: "Gentoo, Portage, Local Mirror, Independence, Digital Survival"
---

All across the internet, in many countries and states, the Gentoo Foundation has established partnerships with companies, universities, governments, and technology labs to host Gentoo mirrors. These mirrors normally contain the Portage tree along with the distfiles that go with that tree. A user is able to connect to the mirror via rsync interface to clone the Portage tree to a local computer. After the user has cloned the tree locally it is possible to view all the software packages that can be accessed through the Portage system.

###Why Setup a Portage Mirror?###

Here are the advantages to setting up a local Portage mirror (in order of most to least relevant):

* Saves bandwidth. 

* Increases in Portage sync speeds.

* Reduces dependence on the Internet and increases independence on infrastructure you control (private infrastructure).


Scenario 1: Every time one of your machines syncs its Portage mirror with an official mirror anywhere between ... and ... MBs of data is used.  These numbers can be multiplied with the amount of machines running sync. Two machines running a sync?

Scenario 2: Local sync speeds are only limited by the speeds of a local area network. It is typical for consumer grade routers to have 100Mbs speeds. "Power users" who buy nice hardware can have 1Gb speeds (those are usually the people who usually have no need to read a guide such as this). Even on a 100Mbs connection the syncs will feel lighting fast compared to getting a sync over the internet.

Scenario 3: Setting up a local Portage mirror enables less dependence upon a internet connection being up all the time. For users located in areas of world that have poor a internet connections (Satellite, spotty ISPs, power conservation) having a local sync server could be a great idea.

Maffblaster

Digital Survival