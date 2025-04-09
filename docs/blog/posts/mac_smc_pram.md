---
title: Reset NVRAM, PRAM or SMC
authors: [silentglasses]
date: 2021-12-22 09:20:00
categories: [Hardware, macOS]
tags: [mac, laptop, build]
---

Non-Volatile Random-Access Memory (NVRAM) and Parameter Random-Access Memory (PRAM) are two crucial types of memory used in modern computers, particularly in Apple's Macintosh systems. These specialized memory types retain important system settings and configuration data even when the computer is turned off, ensuring a smoother and more efficient startup process. NVRAM and PRAM store information such as speaker volume, screen resolution, startup disk selection, and recent kernel panic information.
<!-- more -->

These settings include:

-  sound volume
-  display resolution
-  startup disk selection
-  time zone
-  recent kernel panic information.

If you experience issues related to these settings or others, resetting NVRAM and PRAM might help. To reset these hold down the following keys then power on your MacBook

<kbd>⌥ Option</kbd>+<kbd>⌘ Cmd</kbd>+<kbd>P</kbd>+<kbd>R</kbd>

-  On Mac computers that play a startup sound, you can release the keys after the second startup sound.
-  On Mac computers that have the Apple T2 Security Chip, you can release the keys after the Apple logo appears and disappears for the second time.

### note

-  When your Mac finishes starting up, you might want to open System Preferences and adjust any settings that were reset, such as sound volume, display resolution, startup disk selection, or time zone.
-  If your Mac is using a firmware password, this key combination does nothing or will cause your Mac to start up from macOS Recovery. To reset NVRAM, first turn off the firmware password.

## Reset SMC

The SMC is responsible for:

-  Responding to presses of the power button
-  Responding to the display lid opening and closing on Mac notebooks
-  Battery management
-  Thermal management
-  Sudden Motion Sensor (SMS)
-  Ambient light sensing
-  Keyboard backlighting
-  Status indicator light (SIL) management
-  Battery status indicator lights
-  Selecting an external (instead of internal) video source for some iMac displays

These symptoms might mean that an SMC reset is necessary:

-  Your computer’s fans run at high speed, even though it isn’t under heavy usage and is properly ventilated.
-  The keyboard backlight behaves incorrectly.
-  The status indicator light, if present, behaves incorrectly.
-  Battery indicator lights, if present, behave incorrectly on Mac notebooks with a non-removable battery.
-  The display backlight doesn’t respond correctly to ambient light changes.
-  Your Mac doesn’t respond when you press the power button.
-  Your Mac notebook doesn’t respond properly when you close or open the lid.
-  Your Mac sleeps or shuts down unexpectedly and you can’t turn it back on.
-  The battery doesn’t charge properly.
-  Your MacBook or MacBook Pro doesn’t charge through its built-in USB-C port.
-  Your MacBook or MacBook Pro doesn’t recognize external devices that are connected to its built-in USB-C port.
-  The MagSafe power adapter LED, if present, doesn’t indicate the correct charging activity.
-  Your Mac performs unusually slowly, even though its CPU isn’t under an abnormally heavy load.
-  A Mac that supports target display mode doesn’t switch into or out of target display mode as expected, or it switches into or out of target display mode at unexpected times.
-  The illumination around the I/O ports on a Mac Pro (Late 2013) doesn’t turn on when you move the computer.

### Before resetting your SMC

-  Restart your Mac:  
    -  Apple () menu > Restart
    -  Apple () menu > Shut Down, then press the power button again to turn it on
    -  For battery issues:
        -  Unplug the power adapter from your Mac and the electrical outlet for several seconds, then plug it back in.
        -  Choose Apple () menu > Shut Down and wait for your Mac to shut down.
        -  Press the power button again to turn on your Mac.

### Reset the SMC on computers that have the T2 chip

-  Choose Apple () menu > Shut Down and wait for your Mac to shut down.
-  Press and hold the power button for 10 seconds.
-  Release the power button, then wait a few seconds.
-  Press the power button again to turn on your Mac.

If that doesn’t resolve the issue, follow these steps:

-  Choose Apple () menu > Shut Down and wait for your Mac to shut down.
-  Press and hold the right <kbd>⇧ Shift</kbd> key, the left <kbd>⌥ Option</kbd> key, and the left <kbd>⌃ Ctrl</kbd> key for 7 seconds.
    -  Your Mac might turn on and show the Apple () logo on its display.
    -  Keep holding those keys while you also press and hold the power button for another 7 seconds.
    -  If your Mac turned on when you first pressed the keys, it turns off at this point.
-  Release all three keys and the power button, then wait a few seconds.
-  Press the power button again to turn on your Mac.

### Reset the SMC on other computers

I won't cover the older models with removable batteries.

-  Choose Apple () menu > Shut Down and wait for your Mac to shut down.
-  Press <kbd>⇧ Shift</kbd>+<kbd>⌃ Ctrl</kbd>+<kbd>⌥ Option</kbd> on the left side of the built-in keyboard, then press the power button at the same time.
    -  Hold these keys and the power button for 10 seconds.
    -  If you have a MacBook Pro with Touch ID, the Touch ID button is also the power button.
-  Release all keys.
-  Press the power button again to turn on your Mac.

## Hidden Files

### In Finder

To show hidden files, enter the following key sequence from with finder open:

<kbd>⌘ Cmd</kbd>+<kbd>⇧ Shift</kbd>+<kbd>.</kbd>

Repeat the sequence to hide them again.

### In Terminal

-  Run the following command:

```
defaults write com.apple.Finder AppleShowAllFiles true ; killall Finder
```

-  To undo, run the following command:

```
defaults write com.apple.Finder AppleShowAllFiles false ; killall Finder
```

## Reference

-  [Reset NVRAM or PRAM](https://support.apple.com/en-gb/HT204063)
-  [Resetting the System Management Controller (SMC)](https://support.apple.com/en-gb/HT201295)
