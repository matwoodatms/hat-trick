# Getting ready for a GUI
We're going to use wx to create a GUI python application. To get it, use:
`sudo apt-get install python-wxtools`

# Developing remotely from MacOS
You can use SSH to administer the Raspberry Pi remotely, which is super helpful if you've connected a tiny 240x240 screen...

NOTE: SSH will not be enabled by default in most rpi installs. Use the instructions here to get that enabled: https://stackoverflow.com/questions/41318597/ssh-connection-refused-on-raspberry-pi

With a display connected, you can click Preferences, Interfaces, and enable SSH.

To simplify SSH from MacOS (fewer password entries), use:
`ssh-keygen -t rsa && ssh-copy-id pi@raspberrypi.local`

Originally outlined here: https://github.com/gloveboxes/Create-RaspberryPi-dotNET-Core-C-Sharp-IoT-Applications/blob/master/labs/Lab_1_Build_dot_NET_Core_app/README.md


# Copying files from MacOS to the Raspberry Pi
`scp <Path to File To Copy> pi@raspberrypi.local:<Path that File will Go>`
Example: scp multipleLogos.gif pi@raspberrypi.local:/home/pi/hat-trick/media
