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

Saves bandwidth. 

Increases in Portage sync speeds.

To be less dependent on the internet and more independent on private infrastructure.


Scenario 1: Every time one of your machines syncs its Portage mirror with an official mirror anywhere between ... and ... MBs of data is used.  These numbers can be multiplied with the amount of machines running sync.

Scenario 2: Your local sync speeds are only limited by the speeds of your local area network. It's typical for homes to have 100Mbs connections. Maybe "power users" have 1Gb speeds (those are usually the people who have already setup a local Portage sync server without having to read a guide). Even on a 100Mbs connection your syncs will feel lighting fast compared to normal internet speeds.

Scenario 3: Setting up a local Portage mirror enables you to be less dependent upon your internet connection being up all the time. If you're in an area of world that has poor a internet connection (Satellite, or even just a bad provider).

Maffblaster

Digital Survival