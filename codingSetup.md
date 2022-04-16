# Developing remotely from MacOS
You can use SSH to administer the Raspberry Pi remotely, which is super helpful if you've connected a tiny 240x240 screen...

NOTE: SSH will not be enabled by default in most rpi installs. Use the instructions here to get that enabled: https://stackoverflow.com/questions/41318597/ssh-connection-refused-on-raspberry-pi

This provides a good overview to start coding: https://github.com/gloveboxes/Create-RaspberryPi-dotNET-Core-C-Sharp-IoT-Applications/blob/master/labs/Lab_1_Build_dot_NET_Core_app/README.md


# Getting ready for a GUI
Install a newer version of Python following these steps. I used 3.9.5 as in the sample:
https://raspberrytips.com/install-latest-python-raspberry-pi/#:~:text=To%20update%20Python%20on%20Raspberry%20Pi%2C%20start%20by,is%20up-to-date%3A%20sudo%20apt%20update%20sudo%20apt%20upgrade

The version of pip in the rpi image wasn't up to snuff to install wxPython. I upgraded it, too:
`python -m pip install --upgrade pip`


Then this should work to install wxPython:
`pip install -U wxPython`


https://wiki.wxpython.org/BuildWxPythonOnRaspberryPi

`sudo apt-get install python-wxgtk2.8`

`sudo apt-get install python-wxtools`

Did these after a WHOLE bunch of flailing, including sudo apt-get update and sudo apt-get upgrade...may have screwed things up for the screen. Need to check...
