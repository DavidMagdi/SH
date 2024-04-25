# SH
MySH
If you don't need the onboard audio chip (i.e. analog output or HDMI audio), disable it and then the USB audio device will become the primary audio device. These steps worked on Raspbian Jessie.

Disable onboard audio.
Open /etc/modprobe.d/raspi-blacklist.conf and add blacklist snd_bcm2835.
Allow the USB audio device to be the default device.
Open /lib/modprobe.d/aliases.conf and comment out the line options snd-usb-audio index=-2
Reboot
sudo reboot
Test it out.
$ aplay /usr/share/sounds/alsa/Front_Center.wav
