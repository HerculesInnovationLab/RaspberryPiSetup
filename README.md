## Setup SD card
open "rpi-imager.exe"

Select "CHOOSE OS"
Click "3D printing"
Click "OctoPi"
Click "OctoPi (new camera stack)"

Select "CHOOSE STORAGE"
Select the SD card

Click the gear icon
Check 'Set hostname'
Set hostname to name you would like to use `example.local`
Check 'Configure wireless LAN'
Click "SAVE"

Click "WRITE"

Once finished
Open the SD card called 'boot'
Find "config.txt"
Add `#` before `dtoverlay=vc4-kms-v3d` and `max_framebuffers=2`
	This is needed to fix the touch screen
At the bottom, Add `lcd_rotate=2`
	This sets the LCD output to the right orientation


## Configure Pi
### SSH into Pi
Open "putty" the remote connection application
Connect to whatever hostname you gave it `prusa[X].local`
Username: pi
Password: raspberry

### Update software
run `sudo apt update`
If packages need updated
run `sudo apt upgrade`
hit "y"
hit "enter"
### Disable IPv6
Open `/etc/sysctl.conf` with `sudo nano /etc/sysctl.conf`

At the end of the file, add:
```
net.ipv6.conf.all.disable_ipv6 = 1  
net.ipv6.conf.default.disable_ipv6 = 1  
net.ipv6.conf.lo.disable_ipv6 = 1
```
Type `sudo sysctl -p`
Type `cat /proc/sys/net/ipv6/conf/all/disable_ipv6`
You should see `1`
### Set locales
run `sudo raspi-config`
select "5 Localization Options"
select "L1 Locale"
Uncheck "en_GB.UTF-8 UTF-8" with spacebar
Check "en_US.UTF-8" with spacebar
Hit the tab key to move to "\<Ok\>"
Hit Enter
Select "en_US.UTF-8"

Select "5 Localization Options"
Select "L3 Keyboard"
Select "Generic 104-key PC"
	This is the normal American full-size keyboard, not with the extra ISO key.
Scroll to "Other", Bottom
Select "English (US)"
Scroll to "English (US)" Top
Select "The default for the keyboard layout"
Select "No compose key"

Select "Finish"
### Install desktop
Run `sudo ./scripts/install-desktop`
	This will install the Rasberry Pi desktop
Hit "enter"
Type "yes"
### Install touchscreen keyboard
Run `sudo apt install onboard`
	This is the assistive on screen keyboard

### Install Chromium-Browser
Run `sudo apt install chromium-browser`
	This is a web browser that allows "kiosk mode", needed for full screen.
### Reboot after software installs
reboot with `sudo reboot`
### Setup auto start
Run `mkdir /homme/pi/.config/autostart`
Run `nano /home/pi/Desktop/Prusa[X].desktop`
	input:	```
[Desktop Entry]
Type=Application
Terminal=false
Exec=chromium-browser --start-fullscreen localhost
Name=Prusa[X] Web
Icon=chromium-browser```
	
	Replace \[x\] with the number

Run `nanoo /hoome/pi/Desktop/onboard.desktop`
	input:
```
[Desktop Entry]
Type=Application
Terminal=false
Exec=onboard
Name=onboard
Icon=onboard
```

Link the two with 
`ln -s /home/pi/Desktop/Prusa[X].desktop /hoome/pi/.config/autostart/Prusa[X].desktp`
`ln -s /home/pi/Desktop/onboard.desktop /home/pi/.config/autostart/onboard.desktop`
### Configure Onboard
click start button > Universal Access > Onboard
click notifications icon > Preferences
click "auto-show when editing text"
click "show floating icon when Onboard is hidden"


### Change desktop notifications
Open the folder browser.
Select "Edit" > "Preferences"
Check "Don't ask option no launch executable file"


## Setup Octoprint
open the web interface via the hostname you typed before
"prusa\[X\].local" in your browser

Click "Next"
Click "Next"
Username: "user"
Password: "password"
Confirm same password
Click "Create Account"
Click "Next"
Click "Enable Connectivity Check"
Click "Next"
Click "Enable Anonymous Usage Tracking"
Click "Next"
Click "Enable Plugin Blacklist Processing"
Click "Next"
Leave everything on webcam default
Click "Next"
Type
	Name: "Prusa3"
	Model: "Prusa MK3S+"
Click "Print bed and build volume"
	use settings found [Here](https://help.prusa3d.com/article/octoprint-configuration-and-install_2182)


### Setup Plugins
Once on the webpage
Open the wrench icon for settings
Click "Plugin Manager"
Click "+ Get More"
Search for and install the following
"Cancle Objects"
"ipOnConnect"
"M73 Progress Plugin"
"Slicer Thumbnails"
"Themeify"

### Setup Device, on device
Open "File Manager"
Click "Edit"
Click "Prefferences"
Check "Don't ask options on launch executable file"
Close

Open "onboard"
Click the 3 lines for options in the bottom right
Click the wrench and screw icon on left
Check "Start Onboard hidden"
Check "Show floating icon when Onboard is hidden"
Close

Restart

### Configure OctoPrint
From the same network, load the webpage matching the host name used example: `http://prusa[X].local`
Click "next"
Click "Browse"
Pick the OctoPrint backup file which is a `*.zip` file
Click "Restore"

Once the server restarts, reconnect
Login `user:password`
Click the small wrench in the top right corner.
Click "Printer Profiles"
Click the small pencil icon under "Action"
Change the Name to your Printers name `Prusa[X]`
Click "Confirm"

Click "Application Keys"
Delete the Orca by clicking the small trashcan under "Action"
Click "Proceed"

Type the Application identifier for the slicer 
Click "Generate"
Click the small copy button to the right of the API key

Click "Appearance"
Change "Title" to `Prusa[X]`


### Configure Prusa Slicer
Click the "Printer Settings"

# **Still need to type**



### Turn off screen saver
Plug in a keyboard to the Pi
Press the Pi key (windows key)
Type "Screensaver" open the screensaver setting app
Drop down "Mode:"
Select "Blank Screen Only"


