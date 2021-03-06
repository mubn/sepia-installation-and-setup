# SEPIA Client Installation - Raspberry Pi

## Common Instructions

### 1) Install Raspbian Buster Lite

* Download Raspbian Buster
* Flash MicroSD with Etcher
* Remove MicroSD and replug (to reload filesystem)
* Add an empty file called 'ssh' to the boot folder ([microSD]/boot) to enable SSH
* Add a file called 'wpa_supplicant.conf' with the following content to the boot folder to enable WiFi login (replace 'country' [US, DE, ..], 'ssid' and 'psk'):
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}
```
* Eject the MicroSD and plug it into your RPi
* Connect to your RPi via SSH (in Windows you can use [putty](https://www.putty.org/))
* Finish your RPi setup with `sudo raspi-config` (expand SD card, set timezone, etc.)

### 2) Run SEPIA Client Installation

* Connect your hardware to the RPi (USB mic, ReSpeaker HAT, Hyperpixel Touchscreen, etc.)
* Choose one of the variants below to install your client.

### 3) Run SEPIA Client Setup

* Run `bash setup.sh` from the `~/install` folder
* Set SEPIA server host address (as you would inside your SEPIA app login box)
* Optional: Define a unique device ID (default is 'o1', Android apps have 'a1' and browsers 'b1' by default)
* Optional: Define a new CLEXI-ID (this can be used as password for the remote terminal later, default is: clexi-123)
* Finish your setup by setting automatic login via `sudo raspi-config` (Boot options - Desktop/CLI - Console Autologin)
* Reboot your system 
* Your headless client should automatically start and notify you via a short audio message that he'll be "right there"

### 4) Configure the Client via Remote Connection

* Continue the setup in your [SEPIA Control HUB](https://github.com/SEPIA-Framework/sepia-admin-tools/tree/master/admin-web-tools) by opening the 'client connections' page
* The CLEXI server of your newly installed SEPIA Client should be reachable at `ws://[rpi-IP]:9090/clexi` (via Nginx proxy)
* Enter your CLEXI-ID from the previous step (or use the default) and press the 'CONNECT' button. The remote terminal window at the bottom will show the status of the connection.
* By default your headless client will start the 'setup mode'. This might take a while, depending on your RPi model. You should hear the audio message "ready for setup" at some point
* Use the remote terminal to ping all connected clients by typing `ping all` or use the shortcut button right above the terminal input field
* Your SEPIA Client should answer with client info, device ID and a short message. If this is not the case something went wrong during the setup. Try to reboot your RPi and observe your CLEXI connection status.
* Copy the device ID into the field with the same name (right above the shortcut buttons)
* Use the remote terminal command `call login user [user-ID] password [user-pwd]` (message type: 'SEPIA Client') to login your user
* You should see a "login successful" message in the terminal. If not you can use the command `call ping` to see if the client can reach the SEPIA server. Check your "hostname" settings from the previous step (Client Setup) if ping fails
* Use the command `call test` (message type: 'SEPIA Client') or corresponding shortcut button 'test client' to ... test your client. You should hear an acoustic confirmation
* Reboot your system one last time to finish the configuration (NOTE: your microphone will only have access permission AFTER the reboot)

### 5) Fine Tuning

* Optional: Open the CLEXI settings.json file located at `~/clexi/settings.json` to tweak your client (e.g. activate "Hey SEPIA"). NOTE: please do this AFTER a successful configuration and reboot (previous step)
* Done. Enjoy! :-)

## Variant 1: USB Mic - Speakers via audio jack

Tested with: Raspian Buster, RPi3, RPi4

### Install

* Create installation folder `mkdir -p ~/install` and switch to directory `cd ~/install`
* Download scripts `wget https://github.com/SEPIA-Framework/sepia-installation-and-setup/raw/dev/sepia-client-installation/sepia_client_rpi_raspbian_buster.zip` (TODO: REPLACE WITH MASTER)
* `unzip sepia_client_rpi_raspbian_buster.zip`
* `bash install_sepia_client.sh dev` (TODO: REPLACE WITH MASTER)
* `bash install_usb_mic.sh`
* Reboot the system
* Continue with the step 'SEPIA Client Setup' of the common instruction above

## Variant 2: ReSpeaker 2 Mic HAT - Speakers via ReSpeaker audio jack

Tested with: Raspian Buster, RPi3, RPi Zero

### Install

* Download scripts as described in variant 1 (wget ...)
* Run the ReSpeaker installation first: `bash install_respeaker_mic.sh`
* Continue with variant 1 installation procedure
* DON'T use 'install_usb_mic.sh' at the end ;-)
* Run `bash update_respeaker_boot.sh` to deactivate unused RPi default audio jack
* Reboot the system
* Continue with the step 'SEPIA Client Setup' of the common instruction above

## Variant 3: Hyperpixel 4.0 Touchscreen - USB Mic - Speakers via audio jack

Tested with: Raspian Buster, RPi4

### Install

* Install the Hyperpixel touchscreen first: `curl https://get.pimoroni.com/hyperpixel4 | bash`
* Continue with variant 1 installation procedure
* If you have problems with the touchscreen (swapped axis etc.) run `bash update_hyperpixel4_boot.sh`
* Use `bash setup.sh` to switch between 'display' and 'headless' mode
* Continue with the step 'SEPIA Client Setup' of the common instruction above or configure system via display/touchscreen
