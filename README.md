# Klipper Auto TAP
 Automatically find Voron TAP's probe offset

# Table of Contents
 - [Description](https://github.com/anonoei/klipper_auto_tap#description)
 - [Overview](https://github.com/anonoei/klipper_auto_tap#overview)
 - [Development status](https://github.com/anonoei/klipper_auto_tap#development-statusroadmap)
 - [Using Klipper Auto TAP](https://github.com/anonoei/klipper_auto_tap#using-klipper-auto-tap)
   - [Installation](https://github.com/anonoei/klipper_auto_tap#installation)
   - [Configuration](https://github.com/anonoei/klipper_auto_tap#configuration)
   - [Macro](https://github.com/anonoei/klipper_auto_tap#macro)
   - [Moonraker Update Manager](https://github.com/anonoei/klipper_auto_tap#moonraker-update-manager)

# Description
 - Intent: Create an easy to use, automated way to find Voron TAP's probe offset

# Overview
 - License: MIT

# Development status/roadmap
 - [X] Initial release

# Using Klipper Auto TAP
## Installation
To install this module, you need to clone the repository and run the `install.sh` script
### Automatic installation
```
cd ~
git clone https://github.com/Anonoei/klipper_auto_tap.git
cd klipper_auto_tap
chmod +x install.sh
./install.sh
```
### Manual installation
```
cd ~
git clone https://github.com/Anonoei/klipper_auto_tap.git
cd klipper_auto_tap
ln -sf ~/klipper_auto_tap/auto_tap.py ~/klipper/klippy/extras/auto_tap.py
sudo systemctl restart klipper
```

## Configuration
Place this in your printer.cfg
```
[auto_tap]
```
Optionally, you can include these definitions instead of using the macro arguments
```
[auto_tap]
x: 150
y: 150
stop: 1.0
step: 0.005
set_at_end: True
samples: 5
retract_dist:
```
## Macro
Run the klipper command `AUTO_TAP`. You can also use the arguments below
Argument     | Default | Description
------------ | ------- | -----------
X            | 150     | X position to probe
Y            | 150     | Y position to probe
STOP         | 1.0     | Z height to stop checking
STEP         | 0.0125  | Lift Z by this amount each check
SET          | True    | Set probe offset after calculation
SAMPLES      | 5       | How many times to check
RETRACT      | None    | How far to retract z
PROBE_SPEED  | None    | Speed when probing
LIFT_SPEED   | None    | Speed when lifting
TRAVEL_SPEED | None    | Speed when lifting

## Moonraker Update Manager
```
[update_manager klipper_auto_tap]
type: git_repo
path: ~/klipper_auto_tap
origin: https://github.com/anonoei/klipper_auto_tap.git
managed_services: klipper
```

# How does it work?

 1.  Probe the bed
     - The distance this probe measures is your printer's z0
     - Since TAP presses the nozzle down into the bed, z0 will actuate TAP
 2.  Auto TAP
     - Move the nozzle to z0, raise the nozzle by step until TAP de-actuates
 3. Set z offset to measured distance mean * 2
