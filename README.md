# hat-trick
Directions, links, and code for a wearable, reactive hat.

Largely based on https://www.instructables.com/Connected-Round-Screen-240x240-GC9A01-Controller-t/

## Hardware
GC9A01 round screen - https://www.amazon.com/gp/product/B08VGT2T42/ref=ppx_yo_dt_b_asin_title_o05_s00?ie=UTF8&psc=1
Raspberry Pi 3B

## Raspberry Pi setup
- Follow https://www.raspberrypi.org/documentation/computers/getting-started.html#setting-up-your-raspberry-pi **but** use a custom image from http://downloads.raspberrypi.org/raspbian/images/raspbian-2019-04-09/ (so we can use fbtft flex).
- Power on the Raspberry Pi with a keyboard, mouse, monitor.
- Launch Terminal and...
  - Append three lines to the devices modules
    - `sudo nano /etc/modules`
        ```
        spi-bcm2835
        flexfb
        fbtft_device
        ```
  - Add these statements to a new config file.
    - `sudo nano /etc/modprobe.d/fbtft.conf`
        ```
        options fbtft_device name=flexfb gpios=reset:27,dc:25,cs:8,led:18 speed=40000000 bgr=1 fps=60 custom=1 height=240 width=240`
        options flexfb setaddrwin=0 width=240 height=240 init=-1,0x11,-2,120,-1,0xEF,-1,0xEB,0x14,-1,0xFE,-1,0xEF,-1,0xEB,0x14,-1,0x84,0x40,-1,0x85,0xFF,-1,0x86,0xFF,-1,0x87,0xFF,-1,0x88,0x0A,-1,0x89,0x21,-1,0x8A,0x00,-1,0x8B,0x80,-1,0x8C,0x01,-1,0x8D,0x01,-1,0x8E,0xFF,-1,0x8F,0xFF,-1,0xB6,0x00,0x20,-1,0x36,0x08,-1,0x3A,0x05,-1,0x90,0x08,0x08,0x08,0x08,-1,0xBD,0x06,-1,0xBC,0x00,-1,0xFF,0x60,0x01,0x04,-1,0xC3,0x13,-1,0xC4,0x13,-1,0xC9,0x22,-1,0xBE,0x11,-1,0xE1,0x10,0x0E,-1,0xDF,0x21,0x0c,0x02,-1,0xF0,0x45,0x09,0x08,0x08,0x26,0x2A,-1,0xF1,0x43,0x70,0x72,0x36,0x37,0x6F,-1,0xF2,0x45,0x09,0x08,0x08,0x26,0x2A,-1,0xF3,0x43,0x70,0x72,0x36,0x37,0x6F,-1,0xED,0x1B,0x0B,-1,0xAE,0x77,-1,0xCD,0x63,-1,0x70,0x07,0x07,0x04,0x0E,0x0F,0x09,0x07,0x08,0x03,-1,0xE8,0x34,-1,0x62,0x18,0x0D,0x71,0xED,0x70,0x70,0x18,0x0F,0x71,0xEF,0x70,0x70,-1,0x63,0x18,0x11,0x71,0xF1,0x70,0x70,0x18,0x13,0x71,0xF3,0x70,0x70,-1,0x64,0x28,0x29,0xF1,0x01,0xF1,0x00,0x07,-1,0x66,0x3C,0x00,0xCD,0x67,0x45,0x45,0x10,0x00,0x00,0x00,-1,0x67,0x00,0x3C,0x00,0x00,0x00,0x01,0x54,0x10,0x32,0x98,-1,0x74,0x10,0x85,0x80,0x00,0x00,0x4E,0x00,-1,0x98,0x3e,0x07,-1,0x35,-1,0x21,-1,0x11,-2,12,-1,0x29,-2,2,-3
        ```
  - Make sure this line is uncommented to enable fbtft_device.
    - `sudo nano /boot/config.txt`
        ```
        dtparam=spi=on
        ```
  - `sudo reboot`
    - After restart there should be an fb1 device listed at /dev. 
    - *If there was a failure to load the fbtft_device...*
      - You can check in terminal with `systemctl status systemd-modules-load.service`
      - If it failed, try making sure the fbtft distro is available (see https://forums.raspberrypi.com/viewtopic.php?t=323677)

- Now we need to get things running, copying fb0 to fb1.
  - Install the tools we need.
    - `sudo apt-get install cmake git`
  - Clone the code.
    - ```
      cd ~
      git clone https://github.com/tasanakorn/rpi-fbcp 
      cd rpi-fbcp/ 
      mkdir build 
      cd build/ 
      cmake .. 
      make 
      sudo install fbcp /usr/local/bin/fbcp
      ```
  - Set to auto-run on boot.
    - `sudo nano /etc/rc.local`
      - add
        ```
        sleep 30
        fbcp&
        ```
      - before the "exit 0" at the end. The "&" is necessary to run in the background, or the OS can't boot anymore.
  - Set the display size.
    - `sudo nano /boot/config.txt`
      - Append:
        ```
        hdmi_force_hotplug=1
        hdmi_cvt=300 300 60 1 0 0 0
        hdmi_group=2
        hdmi_mode=1
        hdmi_mode=87
        display_rotate = 1
        ```
  - Now shut down the Raspberry Pi and wire up the screen.
    - ![Wiring diagram](https://content.instructables.com/ORIG/F7J/6R03/KV0YKDNJ/F7J6R03KV0YKDNJ.png?auto=webp&frame=1&width=320&md=a1678bcbbe509f6a766525e2596b4b80)
    
