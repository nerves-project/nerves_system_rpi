# Raspberry Pi Model A+, B, and B+

[![CircleCI](https://circleci.com/gh/nerves-project/nerves_system_rpi.svg?style=svg)](https://circleci.com/gh/nerves-project/nerves_system_rpi)
[![Hex version](https://img.shields.io/hexpm/v/nerves_system_rpi.svg "Hex version")](https://hex.pm/packages/nerves_system_rpi)

This is the base Nerves System configuration for the Raspberry Pi A+, B,
and B+. It will also work with the Raspberry Pi Zero, but usually
[nerves_system_rpi0](https://github.com/nerves-project/nerves_system_rpi0) is
preferable since it configures the USB port so that it can be plugged into
a computer. This is very convenient for powering and debugging the Pi Zero.
If you want to use the Raspberry Pi Zero's USB port in "host" mode (e.g.,
to attach a USB Flash drive or other peripheral), then this repository
works and may be more appropriate.

This is not the configuration for the Raspberry Pi 2 or 3.

![Fritzing Raspberry Pi A+ image](assets/images/raspberry-pi-model-a-plus.png)
<br><sup>[Image credit](#fritzing)</sup>

| Feature              | Description                     |
| -------------------- | ------------------------------- |
| CPU                  | 700 MHz ARM1176JZF-S for A+, B, and B+, 1 GHz ARM1176JZF-S for the Zero  |
| Memory               | 256 MB for rev 1 boards, 512 MB for rev 2 and the Zero  |
| Storage              | MicroSD                         |
| Linux kernel         | 4.4 w/ Raspberry Pi patches     |
| IEx terminal         | HDMI and USB keyboard (can be changed to UART)   |
| GPIO, I2C, SPI       | Yes - Elixir ALE                |
| ADC                  | No                              |
| PWM                  | Yes, but no Elixir support      |
| UART                 | 1 available - `ttyAMA0`         |
| Camera               | Yes - via rpi-userland          |
| Ethernet             | Yes (only on the B and B+)      |
| WiFi                 | Requires USB WiFi dongle        |
| Bluetooth            | Not supported                   |

## Using

The most common way of using this Nerves System is create a project with `mix
nerves.new` and to export `MIX_TARGET=rpi`. See the [Getting started
guide](https://hexdocs.pm/nerves/getting-started.html#creating-a-new-nerves-app)
for more information.

If you need custom modifications to this system for your device, clone this
repository and update as described in [Making custom
systems](https://hexdocs.pm/nerves/systems.html#customizing-your-own-nerves-system)

If you're new to Nerves, check out the
[nerves_init_gadget](https://github.com/nerves-project/nerves_init_gadget)
project for creating a starter project. It will get you started with the basics
like bringing up networking, initializing the writable application data
partition, and enabling ssh-based firmware updates.  It's easiest to begin by
using the wired Ethernet interface 'eth0' and DHCP if you have a model B/B+.

## Supported USB WiFi devices

The base image includes drivers and firmware for Ralink RT53xx
(`rt2800usb` driver) and RealTek RTL8712U (`r8712u` driver) devices.

We are still working out which subset of all possible WiFi dongles to
support in our images. At some point, we may have the option to support
all dongles and selectively install modules at packaging time, but until
then, these drivers and their associated firmware blobs add significantly
to Nerves release images.

If you are unsure what driver your WiFi dongle requires, run Raspbian and
configure WiFi for your device. At a shell prompt, run `lsmod` to see which
drivers are loaded.  Running `dmesg` may also give a clue. When using `dmesg`,
reinsert the USB dongle to generate new log messages if you don't see them.

## Linux kernel and RPi firmware/userland

There's a subtle coupling between the `nerves_system_br` version and the Linux
kernel version used here. `nerves_system_br` provides the versions of
`rpi-userland` and `rpi-firmware` that get installed. I prefer to match them to
the Linux kernel to avoid any issues. Unfortunately, none of these are tagged by
the Raspberry Pi Foundation so I either attempt to match what's in Raspbian or
take versions of the repositories that have similar commit times.

[Image credit](#fritzing): This image is from the [Fritzing](http://fritzing.org/home/) parts library.
