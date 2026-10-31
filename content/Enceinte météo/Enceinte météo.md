---
id: "Enceinte météo"
aliases: []
tags: []
State: "In progress"
---
[[Code enceinte météo]]

# Preface
## Requirements
- 3 modes controlled by a 3 way switch.
  - Mode 1: CD player
  - Mode 2: Bluetooth/Standby
  - Mode 3: Web radio
- Power on/off controlled by a two-way switch
- Volume controlled by two spring switches
## Hardware
- CD Player: XXX, 5v XA
- Amplifier: XinYi, 12-24V, XA
- Computer: Raspberry Pi 3B+, 5v, XA
- Power supply: 12v stabilized XXA

# General
The Raspberry Pi will be flashed with RaspiOS - Lite, as it won't need any graphical interface. **The finished code will be formatted in read only to avoid deterioration in case of abnormal power offs!**

## Audio modes
### Playing the CD:
Playing a track on a CD necessitates a preload as the data has to be extracted from said CD. The radio will then have two tasks to juggle with:
1. Extracting the current track to have a digital file to play.
2. Preloading the next track while the first one is playing.

That way, we can minimize the buffer and still have a smooth listening experience.

### Web radio
The web radio will have a defined source URL, and will only be able to play from the associated source.

### Bluetooth
The radio will always be set to discoverable, and will accept any connection. The Bluetooth mode will essentially be an idle mode, as the pi naturally acts as an emitter - receiver, so no further configuration is needed.

### [[source-select|Source select]]
We will be using a three-way switch to select the audio mode. The switch will be connected to two GPIO input pins and one ground pin, that way the pi will be able to read the state of the switch and select the appropriate output mode.

### [[power-toggle|Power toggle]]
Using a two-way switch on GPIO3, and adding a `shell: systemctl poweroff` to the script. Placing the switch back on the *On* position will then turn the pi back on. It is to be noted that turning the switch back on the *On* position immediately after shutdown will obviously not work.
