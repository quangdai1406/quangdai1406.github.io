---
layout: page
title: Initial Setup
permalink: /operating/initial/
---

These are instructions on setting up the BeagleBone Black for the first time. They only need to be done once. These instructions assume your computer is running Ubuntu 14.04 or similar.

## Installing updated OS image

Get a microSD card that is at least 4 GB

[Download the latest Debian pre-configured image flasher from their website](http://elinux.org/Beagleboard:BeagleBoneBlack_Debian#BBB_Rev_C_.284GB_eMMC.29_4GB_eMMC_Flasher)

Look for a section on the page titled something like this "BBB Rev C (4GB eMMC) 4GB eMMC Flasher"

Follow the instructions to load it onto the SD card using dd

As of 27 April 2015 the steps are:

Download image:

	wget https://rcn-ee.com/rootfs/bb.org/release/2015-03-01/lxde-4gb/BBB-eMMC-flasher-debian-7.8-lxde-4gb-armhf-2015-03-01-4gb.img.xz

Verify image:

	md5sum BBB-eMMC-flasher-debian-7.8-lxde-4gb-armhf-2015-03-01-4gb.img.xz

Output of md5sum should be:

	38bedfc81de00907ff2913b04bdc6fe9

Uncompress the image:

	unxz BBB-eMMC-flasher-debian-7.8-lxde-4gb-armhf-2015-03-01-4gb.img.xz

Load the image onto the uSD card, replace sdX with the correct drive:

	sudo dd if=./BBB-eMMC-flasher-debian-7.8-lxde-4gb-armhf-2015-03-01-4gb.img of=/dev/sdX

Remove the uSD card from your computer, insert it into the BBB, press and hold the boot select button next to the uSD card, and apply external power. All four LEDs will light up, let go of the boot button. They will flash a bit, then make a Cylon pattern while flashing. It should take less than ten minutes. Once it completes the lights will either turn solid or turn completely off, then you can remove power, remove the uSD card, and boot normally.

If only two or three lights light up, it may not have enough power, or dd may have failed. Try a better power supply (>1A) or re-load the image on the uSD card. Do not use USB power, USB does not provide enough current and the flashing will fail.

## First login

SSH into the BBB, no password should be needed:

	ssh -o StrictHostKeyChecking=no -oUserKnownHostsFile=/dev/null root@192.168.7.2

## WiFi

Commands for WiFi must be sudo so do that now:

	sudo su

Ensure the WiFi adapter is connected and has drivers with `lsusb`, you should see it in the list. You can scan for visible networks with `iwlist scan | grep ESSID`. Create a wpa_supplicant file for asu's WiFi:

	nano /etc/wpa_supplicant.conf

with the following contents: (replace ASURITE and PASSWORD with your ASURITE ID and password)

	# EAP-PEAP/MSCHAPv2 configuration for RADIUS servers that use the new peaplabel
	# (e.g., Radiator)
	network={
	        ssid="asu_encrypted"
	        key_mgmt=WPA-EAP
	        eap=PEAP
	        identity="ASURITE"
	        password="PASSWORD"
	#       ca_cert="/etc/cert/ca.pem"
	#       phase1="peaplabel=1"
	        phase2="auth=MSCHAPV2"
	#       priority=10
	}

Create a wpa_supplicant for other networks such as phone hotspots or home networks (with WPA security) with:

	wpa_passphrase YOUR_ESSID YOUR_WPA_KEY > /etc/wpa_supplicant_YOUR_ESSID.conf

Currently it is only possible to connect to the ASU WiFi and networks that use WPA security automatically. If you want to connect to a network that is unsecured or uses WEP, instructions are easily found on the Internet.

Connect to WiFi manually for now, once setup is complete this will happen automatically when the code is run: (replace `wpa_supplicant.conf` with the filename given above if it is a non-ASU network)

	wpa_supplicant -B -d -i wlan0 -c /etc/wpa_supplicant.conf
	dhclient -v -1 wlan0
	ntpdate time.nist.gov

Check that you have internet with `ping 8.8.8.8` and check that you have working DNS with `ping rayes.io`. If you have internet and not working DNS then you may need to manually add the DNS servers, this should not be necessary however.

	echo "namespace 8.8.8.8" > /etc/resolv.conf
	echo "namespace 8.8.4.4" > /etc/resolv.conf

## Install OS packages

Update all the existing software:

	apt-get update
	apt-get dist-upgrade

## Upload code

On your PC in the public2 folder upload the files:

	npm run rsync
	npm run rsyncrepos

On the BBB install the dependencies:

	cd /root/robot
	npm install

## Compile code

The prussdrv package does not compile automatically, do that now:

	cd /root/robot/node_modules/prussdrv
	node-gyp configure
	node-gyp build

Build the device tree overlays and PRU code, this will need re-run whenever one of them changes. When a device tree overlay changes, the BBB will need to be re-started after building it for the changes to take affect.

	cd /root/robot
	npm run build

## Device tree

Edit the file:

	sudo nano /boot/uEnv.txt

Add this line:

	optargs=fixrtc quiet capemgr.disable_partno=BB-BONELT-HDMI,BB-BONELT-HDMIN capemgr.enable_partno=BB-UART5

## Web Interface

On your PC in the public2 folder install the web interface dependencies:

	npm install
