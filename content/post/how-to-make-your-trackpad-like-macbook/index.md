---
# Name of the article
title: "How to Make Your Trackpad Like Macbook"

# Quick description
description: We all agree on Mac's trackpad, nothing to debate about that. Non-Macs can also do that (not really much), and here is how.

# Author of the article
author: Master Pi

# Date created
date: 2021-09-11

# Article's tags
tags: 
    - trackpad
    - macbook
    - mtrack

# Article's categories: Blog, Project or Guideline
categories:
    - Guideline

# Useful to link articles together for "See also" part
series: 

# is Math included? Default: false
math: false

# Cover image of the article
image: cover.jpg

# Appears as the tail of the output URL.
slug: "how-to-make-your-trackpad-like-macbook"
--- 

If you used Mac products before, I bet the only thing you will never forget is its multi-touch trackpad. Its convenience can improve the work performance a lot, and it even made me feel confused when using other products's trackpad. Luckily, when you say ***"Linux"***, you are talking about the ***"Open Sources"***, and there are some drivers that support it, like *`mtrack`*.

In this blog, we will dive into how we can make our trackpad as close to Mac experience as we can get with *`mtrack`*.

## Why ***mtrack***?
In Linux, there are 3 options for mouse driver: *mtrack*, *libinput* and *synaptics*, but the default driver for almost every Linux distro *libinput* doesn't have many options for us to tweak, while *mtrack* has many flexible configurations, and supports a lot of gestures. The only downside of *mtrack* is it's quite [outdate](https://github.com/BlueDragonX/xf86-input-mtrack) (since 2015), so we will have to clone the community's driver from [GitHub](https://github.com/p2rkw/xf86-input-mtrack).

And be noted that *mtrack* supports only <cite>**Xorg** environment.[^1]</cite>
[^1]: It doesn't work on *Wayland* environment, but I'm fine about that because *Xorg* is more popular and has better performance and resource usage.

## Install the driver
First, we need to install the prerequisites for the driver:
```bash
sudo apt update && sudo apt upgrade
sudo apt install build-essential git pkg-config libmtdev-dev mtdev-tools xserver-xorg-dev xutils-dev
```

Install *mtrack* driver:
```bash
cd /tmp
git clone git clone https://github.com/p2rkw/xf86-input-mtrack.git && cd xf86-input-mtrack
./configure --with-xorg-module-dir=/usr/lib/xorg/modules
sudo make
sudo make install
```
If you get stuck on the install progress, leave me a comment down there.
## Configure the new driver
If the install progress is successfull, the driver should be in the system, but we still need to edit the configuration file so that the system could recognize and use it.

### Configuration file
The configuration file gets placed in `/usr/share/X11/xorg.conf.d/50-mtrack.conf` and it contains options for the trackpad too. We will edit this file and <cite>**restart the system**[^2]</cite> to make changes.

[^2]: The only way to make it work is by restarting *X Server*, but you can't just run `startx` or log out and log in again, so my official reccommendation is restarting the system. 
```bash
# You can use other text editor, but it can only be saved with root permission
sudo nano /usr/share/X11/xorg.conf.d/50-mtrack.conf
```
The configuration file should be in this form (mine):
```
# https://github.com/p2rkw/xf86-input-mtrack
Section		"InputClass"
MatchIsTouchpad	"on"
Identifier	"Touchpads"
MatchDevicePath "/dev/input/event*"
Driver		"mtrack"

## Basic
# Disable Trackpad
Option		"TrackpadDisable"		"false"
# Whether or not to enable physical button on trackpad
Option          "ButtonEnable"                  "true"
# If the button is integrated into the trackpad like Macbook Pros, this option has to be "true"
Option          "ButtonIntegrated"              "false"

## Responsiveness
# The faster you move, the more distance pointer will travel, using "polynomial" profile
Option          "AccelerationProfile"		"2"
# Sensitivity controls how fast your cursor will move. 1 is the default
Option          "Sensitivity"                   "1" 
# Define the pressure at which a finger is detected as a touch
Option          "FingerHigh"                    "5"  
# Defines the pressure at which a finger is detected as a release
Option          "FingerLow"                     "5"
# Whether or not to ignore touches that are determined to be thumbs
Option          "IgnoreThumb"                   "true"
# The minimum size of what's considered a thumb.
# It is expected that a thumb will be larger than other fingers
Option          "ThumbSize"                     "25"  
# The width/length ratio of what's considered a thumb.
# It is expected that a thumb is longer than it is wide
Option          "ThumbRatio"                    "70"  
# Whether or not to disable the entire trackpad when a thumb is touching
Option          "DisableOnThumb"                "false"
# Whether or not to ignore touches that are determined to be palms
Option          "IgnorePalm"                    "true"
# Whether or not to disable the entire trackpad when a palm is touching
Option          "DisableOnPalm"                 "false"
# The minimum size of what's considered a palm.
# Palms are expected to be very large on the trackpad
Option          "PalmSize"                      "40"      

## Zones
# Whether or not to enable button zones.
# If button zones are enabled then the trackpad will be
# split into one, two, or three vertical zones
Option		"ButtonZonesEnable"		"false"
# The button to emulate when the zone is pressed.
# This is the leftmost part of the pad.
Option		"FirstZoneButton"		"0"
# The button to emulate when the zone is pressed.
# This will float to the right of the leftmost zone.
Option		"SecondZoneButton"		"0"
# The button to emulate when the zone is pressed.
# This will float to the right of the leftmost zone.
Option		"ThirdZoneButton"		"0"
# Restrict button zones inside the "EdgeBottom" area.
# So instead of enabling zones on the full pad height,
# the zone is limited to the percentage set for the "EdgeBottom".
Option		"LimitButtonZonesToBottomEdge"	"false"

## Physical Click
# Which button to emulate when no valid finger placement
# is touching the trackpad during a click, as on "EdgeBottom".
Option		"ClickFinger0"			"0"
# Which button to emulate when one finger is touching the trackpad during a click.
Option		"ClickFinger1"			"1"
# Which button to emulate when two fingers are touching the trackpad during a click.
Option		"ClickFinger2"			"3"
# Which button to emulate when three fingers are touching the trackpad during a click.
Option		"ClickFinger3"			"2"
# Whether or not to count the moving finger when emulating button clicks.
Option		"ButtonMoveEmulate"		"true"
# How long (in ms) to consider a touching finger as part of button emulation.
Option		"ButtonTouchExpire"		"100"

## Tap
# Which button to emulate for one-finger tapping.
Option          "TapButton1"			"1"
# Which button to emulate for two-finger tapping.
Option          "TapButton2"			"3"
# Which button to emulate for three-finger tapping.
Option          "TapButton3"			"2"
# Which button to emulate for four-finger tapping.
Option		"TapButton4"			"0"
# When tapping, how much time to hold down the emulated button.
Option		"ClickTime"			"25"
# The amount of time to wait for incoming touches
# after first one before counting it as emulated button click.
Option		"MaxTapTime"			"120"
# How far a touch is allowed to move before counting it is no longer considered a tap.
Option		"MaxTapMove"			"400"

## Gesture
# When a gesture triggers a click, how much time to hold down the emulated button.
Option		"GestureClickTime"		"10"
# Touches are allowed to transition from one gesture to another.
# For example, you may go from scrolling to swiping without
# releasing your fingers from the pad. This value is the amount
# of time you must be performing the new gesture before it is triggered.
# This prevents accidental touches from triggering other gestures.
Option		"GestureWaitTime"		"100"

# How far you must move your fingers before a button click is triggered.
Option		"ScrollDistance"		"22"
# For two finger scrolling. How long button triggered by scrolling will be hold down.
Option		"ScrollClickTime"		"12"
# For two finger scrolling. Sensitivity (movement speed) of pointer during two finger scrolling.
Option		"ScrollSensitivity"		"0"
# For two finger scrolling. The button that is triggered by scrolling up.
Option		"ScrollUpButton"		"5"
# For two finger scrolling. The button that is triggered by scrolling down.
Option		"ScrollDownButton"		"4"
# For two finger scrolling. The button that is triggered by scrolling left.
Option		"ScrollLeftButton"		"7"
# For two finger scrolling. The button that is triggered by scrolling right.
Option		"ScrollRightButton"		"6"
# For two finger scrolling. Whether to generate high precision scroll events.
Option		"ScrollSmooth"			"1"
# How long after finished scrolling movement should be continued. Works only with smooth scrolling enabled.
Option		"ScrollCoastDuration"		"500"
# How fast scroll should be to enable coasting feature.
Option		"ScrollCoastEnableSpeed"	"0.1"

# For three finger swiping. How far you must move your fingers before a button click is triggered.
Option		"SwipeDistance"			"1"
# For three finger swiping. How long button triggered by swiping will be hold down.
Option		"SwipeClickTime"		"0"
# For three finger scrolling. Sensitivity (movement speed) of pointer during three finger scrolling.
Option		"SwipeSensitivity"		"1000"
# For three finger swiping. The button that is triggered by swiping up.
Option		"SwipeUpButton"			"1"
# For three finger swiping. The button that is triggered by swiping down.
Option		"SwipeDownButton"		"1"
# For three finger swiping. The button that is triggered by swiping left.
Option		"SwipeLeftButton"		"1"
# For three finger swiping. The button that is triggered by swiping right.
Option		"SwipeRightButton"		"1"

# For four finger swiping. How far you must move your fingers before a button click is triggered.
Option          "Swipe4Distance"                 "700"
# For four finger swiping. How long button triggered by swiping will be hold down.
Option          "Swipe4ClickTime"                "300"
# For for finger scrolling. Sensitivity (movement speed) of pointer during four finger scrolling.
Option          "Swipe4Sensitivity"              "0"
# For four finger swiping. The button that is triggered by swiping up.
Option          "Swipe4UpButton"                 "8"
# For four finger swiping. The button that is triggered by swiping down.
Option          "Swipe4DownButton"               "9"
# For four finger swiping. The button that is triggered by swiping left.
Option          "Swipe4LeftButton"               "10"
# For four finger swiping. The button that is triggered by swiping right.
Option          "Swipe4RightButton"              "11"

# For one finger edge scrolling. How far you must move your finger on edge before a button click is triggered.
Option          "SwipeDistance"                 "700"
# For one finger edge scrolling. How long button triggered by edge scrolling will be hold down.
Option          "SwipeClickTime"                "300"
# For one finger edge scrolling. Sensitivity (movement speed) of pointer during one finger scrolling.
Option          "SwipeSensitivity"              "0"
# For one finger edge scrolling. The button that is triggered by edge scrolling up.
Option          "SwipeUpButton"                 "4"
# For one finger edge scrolling. The button that is triggered by edge scrolling down.
Option          "SwipeDownButton"               "5"
# For one finger edge scrolling. The button that is triggered by edge scrolling left.
Option          "SwipeLeftButton"               "6"
# For one finger edge scrolling. The button that is triggered by edge scrolling right.
Option          "SwipeRightButton"              "7"

# For pinch scaling. How far you must move your fingers before a button click is triggered.
Option		"ScaleDistance"			"300"
# For pinch scaling. The button that is triggered by scaling up.
Option		"ScaleUpButton"			"12"
# For pinch scaling. The button that is triggered by scaling down.
Option		"ScaleDownButton"		"13"

# For two finger rotation. How far you must move your fingers before a button click is triggered.
Option		"RotateDistance"		"150"
# For two finger rotation. The button that is triggered by rotating left.
Option		"RotateLeftButton"		"14"
# For two finger rotation. The button that is triggered by rotating right.
Option		"RotateRightButton"		"15"

## Edge
# The size of an area at the top of the trackpad where new touches are ignored
# (fingers travelling into this area from the bottom will still be tracked).
Option		"EdgeTopSize"			"0"
# The size of an area at the bottom of the trackpad where new touches are ignored
# (fingers travelling into this area from the bottom will still be tracked).
Option          "EdgeBottomSize"		"0"
# The size of an area at the left of the trackpad where new touches are ignored
# (fingers travelling into this area from the bottom will still be tracked).
Option          "EdgeLeftSize"                  "0"
# The size of an area at the right of the trackpad where new touches are ignored
# (fingers travelling into this area from the bottom will still be tracked).
Option          "EdgeRightSize"                 "0"

## Special Features
# For two finger hold-and-move functionality. The button that is triggered
# by holding one finger and moving another one.
Option		"Hold1Move1StationaryButton"	"1"
# For two finger hold-and-move functionality. Fow far stationary finger can be moved berfore gesture invalidation.
Option		"Hold1Move1StationaryMaxMove"	"20"
# Whether or not to enable tap-to-drag functionality.
Option		"TapDragEnable"			"false"
# The tap-to-drag timeout. This is how long the driver will wait
# after a single tap for a movement event before sending the click.
Option		"TapDragTime"			"350"
# How long after detecting movement to trigger a button down event.
# During this time pointer movement will be disabled.
Option		"TapDragWait"			"40"
# How far the finger is allowed to move during drag wait time.
# If the finger moves farther than this distance during the wait time
# then dragging will be canceled and pointer movement will resume.
Option		"TapDragDist"			"200"
# This is how long the driver will wait after initial drag in 'drag ready' state
# in which it will be able to resume previous drag without additional up, down sequence.
Option		"TapDragLockTimeout"		"500"
# Whether or not to invert the X axis.
Option		"AxisXInvert"			"false"
# Whether or not to invert the Y axis.
Option          "AxisYInvert"                   "false"
EndSection
```
It's quite long, but it contains all options for the trackpad with the detail explaination, so you don't need to search google for any other options. My configuration above is as close as my Mac experience as I can get, and you can adjust to suit your need too.

More tweaks, head to the driver's [README](https://github.com/p2rkw/xf86-input-mtrack).
### Disable when typing
As the title, the most annoying thing when using *mtrack* is that it is not disable when typing, if you set option `TapButton1` to `1`. So we will need `dispad`, a daemon from the origion authors of *mtrack*:
```bash
sudo apt install libconfuse-dev libxi-dev
cd /tmp
git clone https://github.com/BlueDragonX/dispad.git && cd dispad
./configure
sudo make
sudo make install
```
After built successfully, start the daemon by typing `dispad`.

### Add more gestures
Currently, *mtrack* supports up to 15 mouse simulations, however, Ubuntu and some other distros only uses mouse buttons up to 9. To make use of other gestures, we will need some tools.
```bash
sudo apt install xbindkeys xdotool
```

`xbindkeys` is a daemon listening to your inputs from any input devices including keyboard, mouse and touchpad. You’ll define a rule at which, if an input or combination of inputs matches, some command can be executed. That’s where `xdotool` come in. `xdotool` is the command line tool which is able to simulate other key inputs. So by combining `xbindkeys` and `xdotool`, we’ll be able to map high mouse buttons (> 9) to certain shortcuts that suit our needs.

To define the rule, we need to edit the configuration file which is at `~/.xbindkeysrc`, and the detail of the tool is here if you need to see: https://wiki.archlinux.org/title/Xbindkeys.

## Conclusion
That’s my setup for multi-touch touchpad, and it is the as closest as Mac experience I can get. However, there still are some issues with *mtrack* driver after a lot of use, which is minor and do not affect the experience much:
- Scroll coast are sometimes sticky and make scroll move quickly in subsequence scroll gestures.
- Dispad sometimes doesn’t detect typing and lets the caret jump.

Thanks for reading, and let me know in the comment if it helps you!