## Ubuntu 12.04 USB wifi setup instructions Linksys AE1200 

## FATAL: Module ndiswrapper not found.

Overview
=======

Using ubuntu 12.04 you will have problems with your USB wifi card installation it simply will not connect (NO BLUE LIGHT). You can go ahead and install the appropriate drivers for a 64 bit system to no avail. Your will not be able to probe for the device and it will not work and detect networks, follow the instructions below to get it to work properly.

TODO: Update ndiswrapper to ver. 1.58, you can check via $ndiswrapper -v from the prompt. Purge 1.57 and install 1.58 or higher!

## Instructions

Purge all ndiswrapper

	sudo apt-get purge ndiswrapper-common ndiswrapper-utils-1.9 ndiswrapper-modules-1.9 ndiswrapper-dkms

Download the file to your desktop here: 
	
	cd ~/Downloads   # I like to create a Downloads directory instead of /tmp
	Download URL:
	http://sourceforge.net/projects/ndiswrapper/files/

Install requirements, make install, and probe once again for ndiswrapper

	sudo apt-get install linux-headers-generic build-essential
	mkdir ~/src
	cd ~/src
	wget http://downloads.sourceforge.net/project/ndiswrapper/testing/ndiswrapper-1.58rc1.tar.gz
	tar -xvf ndiswrapper-1.58rc1.tar.gz
	cd ndiswrapper-1.58rc1
	sudo su
	make
	sudo make install
	exit


## Install the driver

Download the XP driver for the device: http://support.linksys.com/en-us/support/adapters/AE1200

Extract the zip and cd to the folder (probably xp). If you attempt to install the driver with ndiswrapper at this point, you will receive the "couldn't find section "Linksys_AE1200.files.NTamd64"" error. To resolve this, edit the bcmwlhigh5.inf file. Find the section that looks like this:

	[Linksys_AE2500.files.NT]
	AE2500xp.sys,,,6

Underneath it, add this:

	[Linksys_AE1200.files.NTamd64]
    	AE1200xp64.sys,,,6

	[Linksys_AE2500.files.NTamd64]
    	AE2500xp64.sys,,,6

Save and close. Then, if you've already attempted to install the driver with ndiswrapper you'll need to remove it, run:

	sudo ndiswrapper -e bcmwlhigh5

	Then do the install again:

	sudo ndiswrapper -i bcmwlhigh5.inf

Verify with:

	sudo ndiswrapper -l

Now run 

	modprobe ndiswrapper 
	
There should not longer be the error:

	FATAL: Module ndiswrapper not found.

Now plug in the USB device and the blue light should come on.


	I could be wrong, but I think this means whoever wrote these .inf files 
	forgot to include those lines, which I find funny.


