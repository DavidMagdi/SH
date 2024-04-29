# SH
MySH
**************************      init Config raspberrypi   *******************************
********************************************************************************************
sudo raspi-config







**************************      Disable Default Audio card   *******************************
********************************************************************************************

If you don't need the onboard audio chip (i.e. analog output or HDMI audio), disable it and then the USB audio device will become the primary audio device. These steps worked on Raspbian Jessie.

Disable onboard audio.
Open /etc/modprobe.d/raspi-blacklist.conf and add blacklist snd_bcm2835.
Allow the USB audio device to be the default device.
Open /lib/modprobe.d/aliases.conf and comment out the line options snd-usb-audio index=-2
Reboot
sudo reboot
Test it out.
$ aplay /usr/share/sounds/alsa/Front_Center.wav




**************************      upmpdcli    *******************************
https://www.lesbonscomptes.com/upmpdcli/pages/upmpdcli-debian-build.txt
***************************************************************************


#
# In the following, you may have to adjust the version numbers, I don't update this document
# as often as the packages.
#
# Then:

 # You may need to install sudo first if this is a pristine Debian system. Use su to become root
 # then:
 #    apt install sudo
 # Then
 #    /usr/sbin/usermod -G sudo <yourlogin>

 # Import the repository key:
cd
wget https://www.lesbonscomptes.com/pages/lesbonscomptes.gpg
sudo mv lesbonscomptes.gpg /usr/share/keyrings/

 # Install an apt sources list file.
 # - Substitute the appropriate Debian release name in the following:
 # Use upmpdcli-bookworm for i386/amd64 machines, upmpdcli-rbookworm for ARM. This is not important
 # for retrieving the source, but it may be if binary packages become available later
case `arch` in
x86_64|i386) R='';;
*)R=r;;
esac
wget https://www.lesbonscomptes.com/upmpdcli/pages/upmpdcli-${R}bookworm.list
sudo mv upmpdcli-${R}bookworm.list /etc/apt/sources.list.d/

sudo apt update

 # Install preliminary build dependancies

sudo apt install devscripts

 # Build
cd
mkdir build # Or any name you fancy
cd build 

sudo apt build-dep libnpupnp2
mkdir libnpupnp2
cd libnpupnp2
apt-get source libnpupnp2
cd libnpupnp2-5.0.2/
debuild -us -uc
cd ..
sudo dpkg -i *.deb
cd ..

pwd
 # Current dir is now build/

sudo apt build-dep libupnpp7
mkdir libupnpp7
cd libupnpp7
apt-get source libupnpp7
cd libupnpp7-0.23.0
debuild  -us -uc
cd ..
sudo dpkg -i *.deb
cd ..

pwd
 # Current dir is now build/

sudo apt build-dep upmpdcli
mkdir upmpdcli
cd upmpdcli
apt-get source upmpdcli
cd upmpdcli-1.8.1/
debuild  -us -uc
cd ..
sudo dpkg -i upmpdcli_*.deb
# The above only installs the base package. You may want to add some of the plugin packages, some of
# which have additional dependencies

cd ..
pwd
 # Current dir is now build/

 # Only if you want to use songcast:
sudo apt build-dep sc2mpd
mkdir sc2mpd
cd sc2mpd
apt-get source sc2mpd
cd sc2mpd-1.1.12/
debuild  -us -uc
cd ..
sudo dpkg -i *.deb
