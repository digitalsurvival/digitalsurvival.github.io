---
layout: post
title: How to Setup a Local Gentoo Distfiles Mirror
description: This entry shows the advantages in independence gained when having files locally instead of relying on the internet for source files.
date: 2014-10-06 17:01:24 -07:00
tags: "Local Gentoo Distfiles Mirror"
---
### Why setup a local distfiles mirror? ###

Why would you want to setup a local, private distfiles mirror? There are several reasons why a local mirror is a good idea. First, if your access to the internet goes down, a local mirror enables you to still download and install packages. Will they be the absolute newest packages? No. They will be only as up to date as your local repository is, but as long as you have electricity you will have access to them

Another time you may want to setup a local distfiles mirror is if you're working for a company doing specific tests where they will have to pull many packages to new machines. This will save a *main* Gentoo mirror *lots and lots* of bandwidth.

Another advantage is that internet companies, governments, etc. will not be able to see what you're doing so easily. If you have your mirror setup to sync on a regular basis then it will routinely grab the new packages.. That's it. No one but you will know what you've chosen to install on your personal machine(s) if you sync to your local mirror. They will know, however, that you have the newest packages. Does it really matter if anyone else knows what packages are on your local machine? Probably not, but in the case you want that privacy option a local mirror could help. 

Lets cut to the chase and start to build our local Gentoo distfiles mirror!

### Creating a local Gentoo distfiles mirror ###

Before we download the files, lets create a new directory for these files. I chose to put the files in the following directory on my distfiles server, you can do what I did or choose where you want to put them:

<code>mkdir -p /mirror/distfiles</code>

It's important to note before you start downloading the files to a 40 GB partition you'll be downloading a LOT of files. How many? At least 200 gigabytes worth. Plan on letting your system download files for several days (this all depends on the speed of your internet connection; if you have a uber fast connection like FIOS or something, it might only take a few hours).

You can download the files from a mirror using this, or a similar command:

<code>rsync --recursive --links --safe-links --perms --times --omit-dir-times --compress --stats --human-readable --progress --timeout=180 rsync://rsync.name.of.mirror/gentoo/distfiles/ /mirror/distfiles</code>

Each time you've successfully connected to an official rsync mirror you should see a message of the day (MOTD) similar to this one:.

<blockquote>
GEORGIA TECH SOFTWARE LIBRARY

Unauthorized use is prohibited.  Your access is being logged.

GTlib is hosted by the Research Network Operations Center (RNOC) at the
Georgia Institute of Technology in Atlanta, Georgia, USA.

    GT-RNOC - http://www.rnoc.gatech.edu

GTLib is operated as a "best effort" service.  We _strongly_ recommend
against depending on the availability of this service in a critical context.

If you run a publicly accessible mirror, and are interested in
mirroring from us, please contact lxmirror@gtlib.gatech.edu.

If you run a mirror that is not accessible to the public, please mirror
from the modules listed at rsync://rsync.gtlib.gatech.edu.
</blockquote>

Be sure to read the rsync MOTD in its **entirety**. Sometimes the providers of official mirrors will ask the user to connect to a different location if they will be syncing for the purposes of a private, local mirror (which is *exactly* what you would be doing). <u>You want to be sure your local mirror is being nice with how much it talks to the official mirrors.</u> Your mirror will be downloading a lot more data than a normal user will be, especially to get the initial sync.

Right now there are about 68,419 files being hosted on any distfile mirror. All of these files are varying sizes. The largest file (`FlightGear-data`) is around 1.1 GBs. The smallest (`v4l-dvb-saa716x`) being 10 bytes.

### Using rsync to expose the distfiles to other machines ###

Once you have all the files downloaded, it is a simple matter of "exposing" them to other machines using the rsync daemon. Since Gentoo replies upon the rsync command for Portage, it should be already installed on your system. In the rare instance that it is not installed it can be installed using the emerge command:

<code>emerge net-misc/rsync</code>

It is relatively easy to expose any part of your system to the rsync daemon. You can expose the part of your system that contains the distfiles by editing  <code>/etc/rsyncd.conf</code> accordingly:


<blockquote>
# /etc/rsyncd.conf

# Minimal configuration file for rsync daemon
# See rsync(1) and rsyncd.conf(5) man pages for help

# This line is required by the /etc/init.d/rsyncd script
pid file = /var/run/rsyncd.pid
# You may need to reduce the amount of max connections you allow depending on the speed of your network and your mirror's hardware.
max connections = 30
use chroot = yes
# Make sure read only is set!
read only = yes
motd file = /etc/rsyncd.motd
log file = /var/log/rsyncd.log

[distfiles]
        path = /mirror/distfiles
        comment = My local Gentoo distfiles mirror.
        #exclude /portage /packages
</blockquote>

You can setup a "message of the day" for anyone who connects to your mirror to see by editing the rsyncd.motd file:

<code>/etc/rsyncd.motd</code>

<pre>
Thanks to the article on the Digital Survival website I am now downloading software packages from my local Gentoo distfiles mirror!
</pre>

### Configuring local machines to use the local distfiles mirror ###

After you setup the rsync service, the final step is to tell your machines to use the local mirror instead of downloading the distfiles from the internet (you better not forget this step, lest you be foolish). For each machine you will need to modify the <code>GENTOO_MIRRORS</code> variable in <code>/etc/portage/make.conf</code>

Replace the "IP_Address_Of_Mirror" section with the actual IPv4 address of the machine containing your distfiles. Do this to the <code>make.conf</code> for each machine that is supposed to connect to the local mirror.

<pre>
GENTOO_MIRRORS="rsync://IP_Address_Of_Mirror/distfiles"
</pre>

 As long as you are on an internal network, be it a 10.x.x.x network or a 192.168.x.x, you should be able to connect to the mirror. If you're having problems connecting to your machine, then you are going to need to so some digging to determine why. Unfortunately resolving network problems is beyond the scope of this entry.

That's it! You're done! You've become more independent by having source code for literally thousands of packages saved locally. You can now upgrade, reconfigure, install, uninstall software without an internet connection (providing you have electricity to power your machines). Granted, your mirror should be on a schedule to sync with an upstream public mirror in order to have the most up to date files. Maybe that's the content of another upcoming how-to entry... As long as you configure things properly you should very little maintenance to do.

Soon I will add this guide to the [official Gentoo wiki](https://wiki.gentoo.org/wiki/Main_Page) so that the community will be able to find it in wiki formatting instead of having to read it on a blog! I hope you have found this guide to be both helpful and useful.

If you are interesting in helping out with Digital Survival in any way, please contact me via e-mail: Maffblaster at gmail dot com. As always, I'm happy to hear from you.

There's more to come in the future!

Maffblaster

[Digital Survival](http://www.digitalsurvival.us)