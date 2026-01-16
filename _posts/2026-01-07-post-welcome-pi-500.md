---
title: "Welcome to the pi 500+"
---

In the course of pursuing other grander projects, I can rarely help myself from taking on side-quests.

Today's side quest involves my pi 500+ I got for christmas (thanks dad, and especially Pi Store US for 
sending the "plus" even though only the basic version was ordered) and my preoccupation with (physically
and mentally) ergonomic computing setups.

Long story short, I want my TV to serve as a display so I can sit on the couch while I type away.
While a really long HDMI cable would suffice, I decided to complicate things.
The main idea is to have a pi sitting by the TV hooked up over HDMI to serve primarily as the interface with
the TV. Other devices can interface with it to access the TV as a display.

One idea based on this approach is for the TV pi to function as a read-only mirror for a remote terminal
session broadcasted by [`ttyd`](https://github.com/tsl0922/ttyd).
This one seems pretty cool, but I ran across a simpler approach, which is to use my pi 500 as a bluetooth 
keyboard input device for the TV pi.

So, I flashed a new OS for my pi 4B (which led me to realize that my 500+'s pre-flashed OS is bookworm,
i.e. not the latest) and plugged it into the TV.

Next, I installed [`btferret`](https://github.com/petzval/btferret) on the 500 and built the exe with 
`gcc keyboard.c btlib.c -o keyboard` (see blog [post](https://www.cnx-software.com/2025/11/09/using-the-raspberry-pi-500-keyboard-pc-as-a-regular-bluetooth-keyboard/)).
The blogger notes that running the app from an SSH session doesn't work, which is inconvenient.
Further, the super and alt keys are not supported. Based on one observation I made, it appears that 
the signal from pressing super is received by the 500's OS which can cause subsequent keypresses to 
be diverted from the being sent by `btferret` over the bluetooth connection.
Getting it set up was difficult, especially since I only had one display available.
I had to switch the HDMI between the 500 and the 4. 
It took a few tries to get a successful connection and the server on the keyboard died at least once,
though I may have caused that by pressing ESC, which is the expected behavior.
But, it works, although spamming keys can cause a significant lag.

This might be a viable solution if I could get the keyboard input server to start up using a keyboard
shortcut.
To set up the pi connected to the display, I used the `bluetoothctl` utility to scan,
pair, and connect to the 500. Start running keyboard server on the 500, then run `bluetoothctl`
on the other pi. Enter 'scan on', then once you see the 500 (log message "[NEW] Device D3:56:D6:74:33:04 
HID"), enter 'pair D3:56:D6:74:33:04', then 'connect D3:56:D6:74:33:04'. You can stop the flow of logs 
by entering 'scan off'. If you have access to the desktop, it's a little easier to use the bluetooth 
menu launched from the status bar.

One other idea I have in this vein is a terminal setup which uses a phone as the display for the 500
and which connects over bluetooth. This would be convenient in a lot of settings, especially if networking
is unavailable for whatever reason, even just temporarily. A quick search didn't turn up any existing
phone apps that do this, i.e. present the phone as a terminal emulator, but not an input device.
I started looking into the details and one of the keys I identified is choice of bluetooth protocol.
There are two bluetooth protocols, a simple one (that most resembles the traditional transport basis
of a terminal emulator) and a more complex one (BLE). Supposedly only android supports the simpler protocol.
It may not matter too much, but BLE is more complex and therefore may be harder to work with.
Apart from the bluetooth connectivity, this concept is implemented by `ttyd`.
Looking into this idea got me taking a detour into terminal emulation and transport approaches, i.e.
serial vs. network transport (e.g. TCP/IP).
Here's a great [article](https://poor.dev/blog/terminal-anatomy/) on terminal emulation.

The main point of contrast is that network transport chunks the message into packages so that it
can ensure perfect transmission. This is important, especially with wireless or other fault-prone
configurations, but introduces a lot of complexity (although it doesn't really matter because it's been 
implemented a million times).

Part of the bluetooth display vision could include a custom script which uses the LEDs in the keyboard
to facilitate and indicate the bluetooth pairing process. One way or another, it would be fun to find a
use for the keyboard LEDs. Here's the [repo](https://github.com/raspberrypi/rpi-keyboard-config)
for controlling the keyboard LEDs, and here's the raspberry pi keyboard
[docs](https://www.raspberrypi.com/documentation/computers/keyboard-computers.html).
This [article](https://core-electronics.com.au/guides/how-to-customise-your-pi-500-plus-rgb-keyboard-python-scripts-and-custom-effects/) 
has some example code.

Now I need to record some details I may need later.
- establish host naming convention based on device model, i.e. pi4b, pi4cm, pi500. For now, I've done 
this for the 4B, but the 500's hostname is still raspberrypi. Don't know what I chose for the CM4
- easiest remedy to losing the edges of the screen is to pick a different resolution. In bookworm, there's
a standalone display app, but in trixie, display settings are found within control centre.
