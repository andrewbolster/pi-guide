# Automatically Booting into a Website

The simplest method to have a reliable installation / kiosk on a Raspberry Pi is to not have it on the Pi at all! 

If your project is primarily visual, informative, or interactive, it may be simpler to 'host' your installation offsite, and rely on WiFi availability for your on-site installation.

This will be demonstrated in three parts; a very simple "Space Monitor", where we focus on understanding the `autostart` capability talking to an external website, and a more complex, customisable Python Dashboard that tracks a satellite using a Python based API but that is served from the Raspberry Pi itself.

Experience in Python is not necessary as we'll be more interested in using the `autostart` capability than the details of the dashboard itself. 

## Getting Started with `autostart`

The website we're going to use as an example is very simple; [ClockTab.com](http://www.clocktab.com/) is a clock...

The target of this section is to have a powered-off Pi appear with *just* this clock, no bezel or frame, on an attached screen, without any more interaction that providing power to the Pi. (Assuming either a wired or WiFi connection is plugged in/setup)

In this method, we're going to be using the GUI, so if for whatever reason, your Pi *does not* boot directly into the desktop without any user login, change this configuration using `raspi-config` (`3 Boot Options`->`B1 Desktop / CLI`- > `B4 Desktop Autologin`)

Similarly, your Pi should wait for a network connection before booting; change this configuration using `raspi-config` (`3 Boot Options`->`B2 Wait for Network at Boot `- > `Yes`)

First off, we take the 'default' configuration file from the `/etc/`  partition and copy it in to our home configuration.

```bash
cp /etc/xdg/lxsession/LXDE-pi/autostart /home/pi/.config/lxsession/LXDE-pi/autostart
```

Then we can open it up in the editor of your choice and make it look like this: 

```bash

#@xscreensaver -no-splash  # uncomment this line to enable screensaver
@xset s off
@xset -dpms
@xset s noblank
@chromium-browser --incognito --kiosk http://www.clocktab.com/  # load chromium after boot and point to the localhost webserver in full screen mode
```

And that's it! Save the file, and reboot and you've got your very own clock! You can change the URL to whatever you like

## Satellite Dashboard

Requirements: 

```bash
sudo pip3 install dash dash-html-components dash-core-components dash-table
```

Source Reference: [Here](https://dash.plot.ly/live-updates)