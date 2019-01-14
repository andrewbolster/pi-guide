# Automatic Upgrades

## Introduction

For long-running internet connected kiosks/installations, it is useful to set up the Pi so that it automatically makes sure that it has the latest versions of software installed for security and performance reasons. 

In this section we'll set up the Pi to automatically recieve Security updates. 

[Reference Source](https://wiki.debian.org/UnattendedUpgrades)

## Installation

```bash
sudo apt-get install -y unattented-upgrades apt-listchanges
sudo dpkg-reconfigure -plow unattended-upgrades
```

## Configuration

The default installation of `unattended-upgrades` is looking for updates to a Linux distribution called Debian, rather than the specially customised Rasbian that NOOBS installs.

As such, we need to add two lines to it's configuration file `/etc/apt/apt.conf.d/50unattended-upgrades` in the section that starts `Unattended-Upgrade::Origins-Pattern`

```shell
	"origin=Raspbian,codename=${distro_codename},label=Raspbian";
    "origin=Raspberry Pi Foundation,codename=${distro_codename},label=Raspberry Pi Foundation";
```

## Testing

We can test the configuration to make sure it's doing what we expect by executing `unattented-upgrade` with a `-d` debug flag. Below is the expected output for a correct configuration.

```bash
$ sudo unattended-upgrade -d
Initial blacklisted packages:
Initial whitelisted packages:
Starting unattended upgrades script
Allowed origins are: ['origin=Debian,codename=stretch,label=Debian-Security', 'origin=Raspbian,codename=stretch,label=Raspbian', 'origin=Raspberry Pi Foundation,codename=stretch,label=Raspberry Pi Foundation']
pkgs that look like they should be upgraded:
Fetched 0 B in 0s (0 B/s)
fetch.run() result: 0
blacklist: []
whitelist: []
No packages found that can be upgraded unattended and no pending auto-removals
```

