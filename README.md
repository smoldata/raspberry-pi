# Raspberry Pi Setup

## Hardware

* [Raspberry Pi 3](https://www.adafruit.com/products/3055)
* [Case](https://www.adafruit.com/products/2258)
* [Power cable](https://www.adafruit.com/products/1995)
* [MicroSD card](https://www.adafruit.com/products/2693)
* Optional: HDMI monitor/cable, keyboard

## Install Raspbian

The first step is to flash your MicroSD card with the latest version of [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/).

* [Mac instructions](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
* [Linux instructions](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)
* [Windows instructions](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)

## Terminal login

If you have a monitor and cable handy, it's the safest way to login to your pi. Insert the MicroSD card, plug in the HDMI cable and keyboard, and power it on. You should see a bunch of text scrolling by, and eventually it should show you a login prompt.

Username: `pi`
Password: `raspberry`

## Headless login

*Note: this may not work any more, now that SSH isn't enabled by default*

Once your Pi is up and running, plug it into a router with an ethernet cable & track down the IP address from the DHCP client list. If you have the nmap utility installed, you can also search for all the clients serving on port 22:

```
nmap -sV -p 22 192.168.0.1-254
```

Once you have the IP address you can login:

```
ssh pi@[ip address]
```

Password: `raspberry`

## Change the password

Set a new password: `passwd`

Type the current password, then your new password twice. When you type your password, it won't show your typing, which can be surprising if you're not expecting it. Please *remember your password*, it will be annoying later if you forget it.

## Initial config

Run the configuration utility.

```
sudo raspi-config
```

1. Expand the filesystem
2. Change the root password
3. Internationalisation  
  * Change Locale (I chose `en_US-UTF-8 UTF-8`) and select it on the following screen
  * Change Time Zone to UTC
  * Change Keyboard Layout to one that you prefer (I chose `English US` layout, with everything else default)
4. Choose your country's Wi-Fi settings
5. Advanced settings
  * Enable SSH server
  * Change the hostname to `smoldata`
6. Finish (and reboot)

## Setup wifi

Edit the wifi config: `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

Add the following configuration:

```
network={
  ssid="Wifi SSID"
  psk="Wifi password"
}
```

Restart the networking service:

```
sudo service networking restart
```

Wait a moment, and then check that you're actually connected to the Internet: `ping 8.8.8.8` (type *ctrl-C* to stop (Mac users that is an actual "ctrl" key, not "command")). If you get an error message, it could just be that you need to wait a bit longer and try again.

## Install dependencies

```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install -y gpac firmware-linux-nonfree crda
```

Now let's reboot and then we should be ready to install some new things.

```
sudo reboot
```

Wait for the RPi to reboot, then log back in: `ssh pi@[ip address]`
