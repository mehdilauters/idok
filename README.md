What is it ?
============

IDOK (kodi reversed name) is a Go program that allows to serve medias to your Kodi plateform (raspbmc, xbmc...)

You may be able to send video, images and music from your computer.

Idok has got two modes:

* your computer serve media from a standard port (default 8080)
* your computer dig a tunnel and serve media

Installation
============

## Install distribution

Linux users can use the auto-install:

	bash <(wget http://dists.metal3d.org/install-idok.sh -qO -)

Or with curl:

	bash <(curl http://dists.metal3d.org/install-idok.sh)

Check that ~/.local/bin is in your PATH. Then try to call:

	idok -h

If you want to get yourself the packed file for Linux, here are the urls:

* http://dists.metal3d.org/idok-x86_64.gz for 64 bits 
* http://dists.metal3d.org/idok-x686.gz for 32 bits 

That command could not work with MacOSX. You can get the gziped binary there: 
http://dists.metal3d.org/idok-darwin.gz

Then gunzip the binary:

	gunzip idok-darwin.gz

(I need help for Mac because I don't have one and cannot be sure of how to install the command at the right path...)

Windows users can get exe: http://dists.metal3d.org/idok.zip. The "idok.exe" file should be launched from command line (cmd command). 

Windows users (again) may know that there is no graphical interface for the idok tool. Maybe one day...

If you have troubles, please fill an issue. But keep in mind that I don't have any Windows or Mac OSX installation. 

Stream your first media
=======================

## HTTP (default)

The HTTP way is not secured. While you're streaming to Kodi (or XBMC), the media can be accessed by other computer in your network. That's not a big problem while you're not streaming important information (restricted video). 

This solution need to open port on your firewall. 

By default, idok opens 8080 port (http-alternative), but you can specify other port using "-port" option.

At first, you must be sure that the port is opened. To open firewall port on you linux installation:

	firewall-cmd --add-port=8080/tcp

When you will reload firewall, or restart computer, the port will be closed. If you want to keep that port opened:

	firewall-cmd --add-port=8080/tcp --permanent

Then, send media:

	idok -target=IP_OF_KODI_OR_XBMC /path/to/media.mp3

If you've opened other port, you can set it. For example for port 1234:

	idok -port=1234 -target=IP_OF_KODI_OR_XBMC /path/to/media.mp3


## SSH

The SSH way is the easier and more secured way. Easier because you don't have to open port on your computer and only the Kodi instance will be able to access your content.

	idok -ssh -target=IP_OF_RASPBERRY /path/to/media.mp3

Your kodi should open the file.

Pressing CTRL+C should stop media stream and exit program.

**Note: If you compiled yourself, remember to patch go.crypto/ssh package as explained above. Dropbear on raspbmc + crypto package are not compatibles without my patch**

Install from source
===================

**WARNING - Because there is a problem with dropbear ssh server on raspbmc, you should patch go.crypto/ssh package with the patched I made. See:
https://code.google.com/p/go/issues/detail?id=8657**

You can clone repository and compile source code yourself:

	git clone http://git.develipsum.com/metal3d/idok.git
	cd idok
	go build idok.go

Then you can put binary in your PATH:

	mkdir -p ~/.local/bin
	cp idok ~/.local/bin

Options
=======

There are other options that may be usefull:

* -target: kodi instance ip or hostname 
* -login : xbmc or kodi login configured on web interface settings
* -password : xbmc or kodi password configured on web interface settings
* -ssh : If set, idok will dig ssh tunnel to stream content
* -sshuser : if you don't user "pi" user
* -sshpass : if you changed standard password of "pi" user
* -sshport : if you changed standard ssh port or to use other ssh server (default is 22)
* -port : local port for media stream if you don't use ssh tunneling, default is 8080

TODO
====

I should ask dropbear maintainer why the tunneling won't work. Installing openssh-server on raspbmc can be complicated for some users.

Add kodi/xbmc port option (some users changed the default 80) 

