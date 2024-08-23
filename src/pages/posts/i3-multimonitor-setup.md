---
layout: ../../layouts/post.astro
title: "Multi-monitor setup with i3."
pubDate: 2024-08-22
description: "Setting up i3 config files to work with multiple monitors."
author: "innerviewer"
excerpt: 
image:
  src:
  alt:
tags: ["i3wm", "linux", "dotfiles"]
---

# Introduction

I became a Linux user almost one year ago. This whole time I was using only one monitor, but two days ago I bought a second one. 
As I am not using a desktop environment, I had to manually set up the second monitor.
Here is a basic guide to a multi-monitor setup.

## Basic i3 config.

First of all, you need to figure out how your monitors are connected. To do so, run `xrandr` command in your terminal.

This is my output:

```title="xrandr"
Screen 0: minimum 8 x 8, current 5760 x 2160, maximum 32767 x 32767
DVI-D-0 disconnected (normal left inverted right x axis y axis)
HDMI-0 connected primary 3840x2160+1920+0 (normal left inverted right x axis y axis) 609mm x 349mm
   3840x2160     60.00*+  59.94    50.00    30.00    29.97    25.00    23.98
   2560x1440     59.95
   2048x1280     60.00
   2048x1080     24.00
   1920x1080     60.00    59.94    50.00    29.97    25.00    23.98    60.00    50.04    50.04
   1600x1200     60.00
   1600x900      60.00
   1280x1024     75.02    60.02
   1280x720      59.94    50.00
   1152x864      75.00
   1024x768      75.03    60.00
   800x600       75.00    60.32
   720x576       50.00
   720x480       59.94
   640x480       75.00    59.94    59.93
DP-0 connected 1920x1080+0+0 (normal left inverted right x axis y axis) 527mm x 296mm
   1920x1080     60.00 + 165.00*  144.00   119.88    59.94    50.00
   1680x1050     59.95
   1440x900      59.89
   1440x576      50.00
   1440x480      59.94
   1280x1024     75.02    60.02
   1280x960      60.00
   1280x720      60.00    59.94    50.00
   1152x864      75.00
   1024x768      75.03    70.07    60.00
   800x600       75.00    72.19    60.32    56.25
   720x576       50.00
   720x480       59.94
   640x480       75.00    72.81    59.94    59.93
DP-1 disconnected (normal left inverted right x axis y axis)
DP-2 disconnected (normal left inverted right x axis y axis)
DP-3 disconnected (normal left inverted right x axis y axis)
DP-4 disconnected (normal left inverted right x axis y axis)
DP-5 disconnected (normal left inverted right x axis y axis)
```

Here, you can see that `HDMI-0` is my main monitor with 4k resolution.
And `DP-0` is a second Full HD monitor.

If your monitors are connected using the same interfaces (DisplayPort, HDMI) and have same resolution and refresh rate, 
you can figure out which one is your main by disabling one of them:

```title="Disabling a monitor with xrandr."
xrandr --output DP-0 --off
```
where you can replace DP-0 by the monitor you need to disable.


Then, you can start editing your i3 config.
You can just copy and paste the sample below to your config.


Things to configure yourself: 
1. Monitor variables and Mod keys.
2. Resolution, refresh rate, scale and DPI.

(lines are highlighted)

```sh title="~/.config/i3/config" showLineNumbers {1, 2, 4, 5, 8, 9}
set $fm HDMI-0
set $sm DP-0

set $mod Mod4
set $alt Mod1


exec_always --no-startup-id xrandr --output $fm --mode 3840x2160 --rate 60.00 --primary --scale 1x1 --dpi 163 &
exec_always --no-startup-id xrandr --output $sm --mode 1920x1080 --rate 165 --left-of HDMI-0 --scale 1x1 --dpi 92 &

# Workspaces
workspace 1 output $fm
workspace 2 output $fm
workspace 3 output $fm
workspace 4 output $fm
workspace 5 output $fm
workspace 6 output $fm
workspace 7 output $fm
workspace 8 output $fm
workspace 9 output $fm
workspace 10 output $fm

workspace 11 output $sm
workspace 12 output $sm
workspace 13 output $sm
workspace 14 output $sm
workspace 15 output $sm
workspace 16 output $sm
workspace 17 output $sm
workspace 18 output $sm
workspace 20 output $sm

set $ws1 "1"
set $ws2 "2"
set $ws3 "3"
set $ws4 "4"
set $ws5 "5"
set $ws6 "6"
set $ws7 "7"
set $ws8 "8"
set $ws9 "9"
set $ws10 "10"

set $ws11 "11"
set $ws12 "12"
set $ws13 "13"
set $ws14 "14"
set $ws15 "15"
set $ws16 "16"
set $ws17 "17"
set $ws18 "18"
set $ws19 "19"
set $ws20 "20"

# switch to workspace
bindsym $mod+1 workspace number $ws1
bindsym $mod+2 workspace number $ws2
bindsym $mod+3 workspace number $ws3
bindsym $mod+4 workspace number $ws4
bindsym $mod+5 workspace number $ws5
bindsym $mod+6 workspace number $ws6
bindsym $mod+7 workspace number $ws7
bindsym $mod+8 workspace number $ws8
bindsym $mod+9 workspace number $ws9
bindsym $mod+0 workspace number $ws10

# move focused container to workspace
bindsym $mod+Shift+1 move container to workspace number $ws1
bindsym $mod+Shift+2 move container to workspace number $ws2
bindsym $mod+Shift+3 move container to workspace number $ws3
bindsym $mod+Shift+4 move container to workspace number $ws4
bindsym $mod+Shift+5 move container to workspace number $ws5
bindsym $mod+Shift+6 move container to workspace number $ws6
bindsym $mod+Shift+7 move container to workspace number $ws7
bindsym $mod+Shift+8 move container to workspace number $ws8
bindsym $mod+Shift+9 move container to workspace number $ws9
bindsym $mod+Shift+0 move container to workspace number $ws10

# switch to workspace
bindsym $alt+1 workspace number $ws11
bindsym $alt+2 workspace number $ws12
bindsym $alt+3 workspace number $ws13
bindsym $alt+4 workspace number $ws14
bindsym $alt+5 workspace number $ws15
bindsym $alt+6 workspace number $ws16
bindsym $alt+7 workspace number $ws17
bindsym $alt+8 workspace number $ws18
bindsym $alt+9 workspace number $ws19
bindsym $alt+0 workspace number $ws20

# move focused container to workspace
bindsym $alt+Shift+1 move container to workspace number $ws11
bindsym $alt+Shift+2 move container to workspace number $ws12
bindsym $alt+Shift+3 move container to workspace number $ws13
bindsym $alt+Shift+4 move container to workspace number $ws14
bindsym $alt+Shift+5 move container to workspace number $ws15
bindsym $alt+Shift+6 move container to workspace number $ws16
bindsym $alt+Shift+7 move container to workspace number $ws17
bindsym $alt+Shift+8 move container to workspace number $ws18
bindsym $alt+Shift+9 move container to workspace number $ws19
bindsym $alt+Shift+0 move container to workspace number $ws20
```

Of course, there is a much prettier and easier way to bind all the keys with `sxhkd`.
Unfortunately, I ran into some problems trying to configure it, but I'll update this post once I figure it out.