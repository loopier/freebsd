# How to FreeBSD

## Wifi

[Wireless Networking - Quick Start](https://docs.freebsd.org/en/books/handbook/advanced-networking/#network-wireless-quick-start)

Connecting a computer to an existing wireless network is a very common situation. This procedure shows the steps required.

Obtain the SSID (Service Set Identifier) and PSK (Pre-Shared Key) for the wireless network from the network administrator.

Identify the wireless adapter. The FreeBSD GENERIC kernel includes drivers for many common wireless adapters. If the wireless adapter is one of those models, it will be listed in the sysctl(8) net.wlan.devices variable:

    sysctl net.wlan.devices

If a wireless adapter is not listed, an additional kernel module might be required, or it might be a model not supported by FreeBSD.

This example shows the Atheros ath0 wireless adapter.

#### WARNING: following is the original info, but a more secure way is described below
Add an entry for this network to /etc/wpa_supplicant.conf. If the file does not exist, create it. Replace myssid and mypsk with the SSID and PSK provided by the network administrator.

    network={
        ssid="myssid"
        psk="mypsk"
    }
 
#### More secure way to add network

To encrypt password use `wpa_supplicant`:

    wpa_passphrase "SSID" "WPA2key" >> /etc/wpa_supplicant.conf 

Add entries to /etc/rc.conf to configure the network on startup:

    wlans_ath0="wlan0"
    ifconfig_wlan0="WPA SYNCDHCP"

Restart the computer, or restart the network service to connect to the network:

    service netif restart

# Window Manager

## Install X
Follow instructions in [Installing Xorg](https://docs.freebsd.org/en/books/handbook/x11/#x-install)

## Install drivers

install `nvidia-driver` and configure the system as described in [NVIDIA](https://docs.freebsd.org/en/books/handbook/x11/#x-configuration-nvidia):

	# sysrc kld_list+=nvidia

but also add this

	# sysrc kld_list+=nvidia-modeset

Create a file `/usr/local/etc/X11/xorg.conf.d/20-nvidia.conf`

	Section "Device"
		Identifier "Card0"
		Driver     "nvidia"
	EndSection

## Install WM

Any will do. `openbox` is easy to start but needs some configuration to do basic stuff.

- install `firefox` and `alacritty`
- copy `/usr/local/share/xdg/openbox/menu.xml` to `.config/openbox` and add `alacritty` to `terminals`
- add `openbox-session` to `xinit.rc`

## StartX

	$ startx 
