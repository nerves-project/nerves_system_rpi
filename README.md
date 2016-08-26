# Raspberry Pi Model A+, B, B+, and Zero
[![Build Status](https://travis-ci.org/nerves-project/nerves_system_rpi.png?branch=master)](https://travis-ci.org/nerves-project/nerves_system_rpi)

This is the base Nerves System configuration for the Raspberry Pi A+, B,
B+ and Zero. This is not the configuration for the Raspberry Pi 2 or 3.

![Fritzing Raspberry Pi A+ image](assets/images/raspberry-pi-model-a-plus.png)
<br><sup>[Image credit](#fritzing)</sup>

| Feature              | Description                     |
| -------------------- | ------------------------------- |
| CPU                  | 700 MHz ARM1176JZF-S for A+, B, and B+, 1 GHz ARM1176JZF-S for the Zero  |
| Memory               | 256 MB for rev 1 boards, 512 MB for rev 2 and the Zero  |
| Storage              | MicroSD                         |
| Linux kernel         | 4.1 w/ Raspberry Pi patches     |
| IEx terminal         | HDMI and USB keyboard (can be changed to UART)   |
| GPIO, I2C, SPI       | Yes - Elixir ALE                |
| ADC                  | No                              |
| PWM                  | Yes, but no Elixir support      |
| UART                 | 1 available - ttyACM0           |
| Camera               | Yes - via rpi-userland          |
| Ethernet             | Yes                             |
| WiFi                 | Requires USB WiFi dongle        |
| Bluetooth            | Not supported                   |

## Supported USB WiFi devices

The base image includes drivers and firmware for Ralink RT53xx
(`rt2800usb` driver) and RealTek RTL8712U (`r8712u` driver) devices.

We are still working out which subset of all possible WiFi dongles to
support in our images. At some point, we may have the option to support
all dongles and selectively install modules at packaging time, but until
then, these drivers and their associated firmware blobs add significantly
to Nerves release images.

If you are unsure what driver your WiFi dongle requires, run Raspbian and configure WiFi
for your device. At a shell prompt, run `lsmod` to see which drivers are loaded.
Running `dmesg` may also give a clue. When using `dmesg`, reinsert the USB
dongle to generate new log messages if you don't see them.

## Accessing IEx terminal via UART / FTDI Cable / `screen`

By default, the raspberry's tty is configured to be available at the HDMI
output. Another option is to have a FTDI Cable connecting `raspberry <-> host`,
so you can see the board's output in your own computer. In order to do this,
you will first have to configure the board's tty to be redirected to the
UART pins:
* make a copy of the `cmdline.txt` file and put it under your project's
`config` folder
* edit it, adding ` -c ttyAMA0` to the end of the _line_
* make a copy of the `fwup.conf` file and put it in the `config` folder
* edit the `file-resource cmdline.txt` directive inside `fwup.conf` to make it
point to your new `cmdline.txt` file. In the end it shall look like this:
```
file-resource cmdline.txt {
    host-path = "${NERVES_APP}/config/cmdline.txt"
}
```
* edit your project's `config.exs` file, indicating that Nerves should look at
your new, modified files. Make it look like this:
```
config :nerves, :firmware,
  fwup_conf: "config/fwup.conf"
```
* after this, on loading the new firmware in the board, you shall be able to
connect to its tty via something like this: `screen /dev/ttyUSB0 115200`.


## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed as:

  1. Add nerves_system_rpi to your list of dependencies in `mix.exs`:

        def deps do
          [{:nerves_system_rpi, "~> 0.6.1"}]
        end

  2. Ensure nerves_system_rpi is started before your application:

        def application do
          [applications: [:nerves_system_rpi]]
        end

[Image credit](#fritzing): This image is from the [Fritzing](http://fritzing.org/home/) parts library.
