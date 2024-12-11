# Useful Commands

## Setup Inverse Mouse Scroll
```bash
xinput set-prop 12 "libinput Natural Scrolling Enabled" 1    
```
<br><br>

## Camera Setup on Linux (Manjaro)

### Listing Video Devices
To list all connected video devices (e.g., integrated cameras, USB webcams):
```bash
v4l2-ctl --list-devices
```

### Identifying USB Cameras
Use `lsusb` to list all USB devices and identify the webcam by its vendor and product ID:
```bash
lsusb
```

### Creating a Symlink to Set USB Camera as Default
To set a USB camera as the default (e.g., `/dev/video0`):
1. Check existing video devices:
    ```bash
    ls /dev/video*
    ```
2. Remove the default integrated camera device:
    ```bash
    sudo rm /dev/video0
    ```
3. Create a symlink from the USB camera (e.g., `/dev/video2`) to `/dev/video0`:
    ```bash
    sudo ln -s /dev/video2 /dev/video0
    ```
The changes are temperory by default, may not work if the device is disconnected and reconnected or after reboot.

<br><br><br>

## Battery Setup
- Create a `bin` folder in `home` directory, create a new file `toggle_power.sh` and paste the below code in it.
``` bash
#!/bin/bash

conservation_mode_path="/sys/bus/platform/drivers/ideapad_acpi/VPC2004:00/conservation_mode"
current_state=$(cat "$conservation_mode_path")

if [ "$current_state" -eq 0 ]; then
    echo 1 > "$conservation_mode_path"
    echo "Conservation mode enabled."
else
    echo 0 > "$conservation_mode_path"
    echo "Conservation mode disabled."
fi
```
- Open terminal and type below command to give it executable permission.
``` bash
chmod +x toggle_power.sh
```

- To change power settings, simply run this file using command
``` bash
~/bin/toggle_power.sh
```

<br><br><br>

# Free up Memory
- Remove cache for not installed packages and all delete packages that are not required by any program.
- Also limit the journalctl size to 50mb.

```bash
sudo pacman -Sc
sudo pacman -Qdt
sudo pacman -Rns $(pacman -Qtdq)
sudo journalctl --vacuum-size=50M
```