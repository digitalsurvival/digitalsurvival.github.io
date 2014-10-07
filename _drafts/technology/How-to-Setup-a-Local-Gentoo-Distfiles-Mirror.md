---
layout: post
title: How to Setup a Local Gentoo Distfiles Mirror
description: This article shows the advantages in independence gained when having files locally instead of relying on the internet for source files.
date: 2014-10-06 17:01:24 -07:00
tags: "Local Gentoo Distfiles Mirror"
---

Why would you want to setup a local, private Distfiles mirror? There's several reasons why this would be a good idea. First, if your access to the internet goes down, you are enabled to still download and install packages from source. Will they be the absolute newest packages? No. They will be only as up to date as your repository was.

If you're doing this for a company doing tests, as I did, you will save the main mirror lots and lots of bandwidth.

Another advantage is that internet companies, governments, etc. will not be able to see what you're doing so easily if you have your mirror setup to sync on a regular basis. Your mirror will routinely grab the packages that you will need. That's it. No one but you will know what you've chosen to install on your personal machine(s).

Before we download the files, lets create a new directory for these files:

<code>mkdir -p /mirror/distfiles</code>

You can really put the distfiles anywhere you want, however it's important to note you'll be downloading a LOT of files. How much? Around 200 GBs. Plan on letting your system download files for several days, depending on the speed of your internet connection.

You can download the files from a mirror using something like this command:

<code>rsync --recursive --links --safe-links --perms --times --omit-dir-times --compress --stats --human-readable --progress --timeout=180 rsync://rsync.name.of.mirror/gentoo/distfiles/ /mirror/distfiles</code>

<pre>
Each time you've successfully connected to an official rsync mirror you should see a message similar to this one:

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
</pre>

Be sure to read the message in its entirety. Sometimes the maintainers of the official mirror will ask the users to connect to a different address if they will be syncing for the purposes of a private, local mirror. You want to be sure your local mirror is being nice with how much it talks to the official mirror.

Right now there is about 68,419 files being hosted on any Distfiles mirror. All of these are varying sizes. The largest file (FlightGear-data) is around 1.1 GBs. The smallest (v4l-dvb-saa716x) being 10 bytes.

## Using Rsync to Expose the Distfiles ##

Once you have all the files downloaded, it is a simple matter of "exposing" them to other machines using the rsync daemon in order to have a local distfiles mirror. Since Gentoo replies upon the rsync command for Portage, it should be already installed on your system. In the uber rare instance that it is not installed it can be installed using the <code>emerge</code> command:

<code>emerge net-misc/rsync</code>

It is relatively easy to expose any part of your system do the rsync daemon. You can expose the part of your system that contains the Distfiles by editing the following file accordingly:

<code>/etc/rsyncd.conf</code>

<pre>
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
        comment = Your local Gentoo Distfiles Mirror
        #exclude /portage /packages
</pre>

You can setup a "message of the day" for anyone who connects to your mirror to see by editing the rsyncd.motd file:

<code>/etc/rsyncd.motd</code>

<pre>
You are downloading files from your local Gentoo Distfiles mirror!
</pre>

## Configuring Local Machines to use the Local Distfiles Mirror ##

After you setup the rsync service, the final step is to tell your machines to use the local mirror instead of downloading the Distfiles from the internet. For each machine you will need to modify the <code>GENTOO_MIRRORS</code> variable in <pre>/etc/portage/make.conf</pre>. Replace the "IP_Address_Of_Mirror" section with the actual IP address of the machine containing the Distfiles. Do this for each machine that is supposed to connect to the local mirror.

<pre>
GENTOO_MIRRORS="rsync://IP_Address_Of_Mirror/distfiles"
</pre>

That's it! You're done! You've become more independent by having source code for thousands of packages saved locally. You can now upgrade, reconfigure, install, uninstall software without an internet connection. Granted, your mirror should be on a schedule to sync with an upstream public mirror in order to have the most up to date files. Maybe that's the content of another upcoming how-to entry... As long as you configure things properly you should very little maintenance to do.

Soon I will add this guide to the Gentoo wiki so that the community will be able to find it in wiki formatting. It might help with some of the clarity.

Maffblaster
Digital Survival