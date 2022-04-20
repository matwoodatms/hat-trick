# Getting ready for a GUI
We're going to use wx to create a GUI python application. To get it, use:

`sudo apt-get install python-wxtools`

`sudo apt-get install python-wxgtk-media3.0`

# Getting ready for Bluetooth
Sadly some combination of all that follow were necessary to get bluetooth working on the RPi.
`sudo apt-get install bluetooth bluez libbluetooth-dev python-bluez`

`python -m pip install pybluez`

`sudo pip3 install pybluez`

This was definitely necessary to get past the "no advertisable device" error.
`cd /etc/systemd/system/bluetooth.target.wants/`

`sudo nano bluetooth.service`

On the ExecStart line with bluetoothd add "-C" and save.

`sudo reboot`

`sudo sdptool add SP`

`sudo hcitool dev`

Note the hci value (for me, hci0).

`sudo hciconfig hci0 piscan`

Now the bluetooth .py should run with sudo successfully.


# Developing remotely from MacOS
You can use SSH to administer the Raspberry Pi remotely, which is super helpful if you've connected a tiny 240x240 screen...

NOTE: SSH will not be enabled by default in most rpi installs. Use the instructions here to get that enabled: https://stackoverflow.com/questions/41318597/ssh-connection-refused-on-raspberry-pi

With a display connected, you can click Preferences, Interfaces, and enable SSH.

To simplify SSH from MacOS (fewer password entries), use:
`ssh-keygen -t rsa && ssh-copy-id pi@raspberrypi.local`

Originally outlined here: https://github.com/gloveboxes/Create-RaspberryPi-dotNET-Core-C-Sharp-IoT-Applications/blob/master/labs/Lab_1_Build_dot_NET_Core_app/README.md


# Copying files from MacOS to the Raspberry Pi
First and foremost with VSCode connected you can just drag/drop to the project folder. But if there's need outside of that:
`scp <Path to File To Copy> pi@raspberrypi.local:<Path that File will Go>`

Example: scp multipleLogos.gif pi@raspberrypi.local:/home/pi/hat-trick/media
