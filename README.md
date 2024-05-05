# Homeassistant-wallpannel-V2

# Touch Screen
This is my (22nd) attempt of home control panel 

## Used Hardware:
- [Raspberry Pi 4 8GB](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/)
- [Raspberry Pi 4 Case (With Cooling Fan) (v3.0)](https://www.thepihut.com/products/raspberry-pi-4-case-with-cooling-fan?variant=31907267706942)
- [Raspberry Pi 15.3W USB-C Power Supply](https://www.raspberrypi.org/products/type-c-power-supply/)
- [UPERFECT y Portable Monitor With FHD 1080P Touch Screen](https://www.aliexpress.com/item/1005002828837342.html?spm=a2g0s.9042311.0.0.4b304c4dO7abT1)

# How-To
- use raspberry pi imager
- select os in my case Raspberry PI OS (Legacy, 64bit) Bullseye
- shift+ctl+x
- check diasble overscan
- set hostname
- enable ssh
- enable vnc
- (optional) configure WIFI
- set locale settings
- Flash 

## install rasbian bullseye
```
sudo apt update && sudo apt upgrade -y
```
```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```
```
@lxpanel --profile LXDE-pi
@pcmanfm --desktop --profile LXDE-pi
@xscreensaver -no-splash
@xrandr --output HDMI-1 --rotate right
@chromium-browser --start-fullscreen --start-maximized --kiosk --noerrdialogs --disable-default-apps --disable-single-click-autofill --disable-translate-new-ux --disable-translate --disable-cache --disk-cache-dir=/dev/null --disk-cache-size=1 --reduce-security-for-testing --enable-features=OverlayScrollbar --touch-events --noerrdialogs --disable-infobars --kiosk http://ha-main.local:8123/kiosk-tablet/home?BrowserID=Kiosk
```
```
sudo nano /etc/lightdm/lightdm.conf
```
add under seat section
```
[Seat:*]
xserver-command=X -s 0 dpms
xserver-command=X -nocursor
```
## Rotate Touch digitalizer
Navigate to
```sudo nano /usr/share/X11/xorg.conf.d/40-libinput.conf```

Find the 
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        .......
```
And replace with
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchscreen "on"
        Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```
```
sudo reboot now
```
