# ChatterSync - Network File Transfer of USB for Un-Networked CNC Machines

## Overview
This project is a simple file transfer system to improve the program transfer capabilites of CNC machines that do not have networking capabilities (pre-NGC Haas, for example). We've developed a device that that connects to the machine's USB port and behaves as a flash drive, but also is accessible over the WiFi network for file transfer. This allows the machine to be connected to the network without any modifications to the machine itself.

We've adopted a "DIY" model for this. Existing systems to do similar things are prohibitavely expensive, so we provide you all the resources to get it going yourself for about $30 per machine.

## Bill of Materials
* [Raspberry Pi Zero W](https://rpilocator.com/?cat=PIZERO%2CPIZERO) - $15.00
* [MicroSD Card (16GB)](https://www.amazon.com/dp/B073K14CVB/) - $8.47
* [MicroUSB to USB-A Cable](https://www.amazon.com/Amazon-Basics-Charging-Transfer-Gold-Plated/dp/B0711PVX6Z/) - $6.64
* (Optional) 3D Printed Case (STEP files included in this repo)

Total cost: $30.11

## Installation
1. Download or clone this repository to your computer for all the files you'll need.
2. Download the latest version of the [Raspberry Pi Imager](https://www.raspberrypi.org/software/) and install it on your computer.
3. Insert your MicroSD card into your computer and open the Raspberry Pi Imager. Select the MicroSD card as the target and select the provided `chattersync.img` file as the source. Click "Write" to write the image to the card.
4. Configure your WiFi network by creating a `wpa_supplicant.conf` file in the root directory of the MicroSD card mounted on your computer. See section below labeled "WiFi Setup" for more details. An example `wpa_supplicant.conf` is located in this repository at `/resources/wpa_supplicant.conf`.
5. Eject the MicroSD card and insert it into the Raspberry Pi Zero W. Connect the Raspberry Pi to your CNC machine using the MicroUSB to USB-A cable plugged into the center MicroUSB port (marked `USB`, not `PWR IN`).
6. Note the IP address of the Raspberry Pi. You can find this by logging into your router and looking at the list of connected devices. Alternatively, you can use a tool like `nmap` or [Advanced IP Scanner](https://www.advanced-ip-scanner.com/) to scan your network for the Raspberry Pi. The hostname of the Raspberry Pi is `chattersync`.
7. Open the Visual Studio Code with the [Chatter NC Editor Extension](https://marketplace.visualstudio.com/items?itemName=chatter-dev.chatter-nc-editor) installed and select "Add Machine".
8. Follow the prompts, using the IP address of the Raspberry Pi as the "Machine Address". The "Machine Name" can be whatever you want to call it. For "protocol", select "sftp" (this will default to port 22). This is the most secure option. For "username", enter `chatter`, and for "password", enter `chatter`. These are the default credentials for this image.
9. Once you have added the machine, click the settings cogwheel and change the property `basePath` to `/mnt/chattersync`. This will allow you to access the USB emulation volume over the network.
10. You should now be able to connect to the Raspberry Pi and transfer files to your CNC machine.

## Important Notes - **Don't Skip This Part!**
1. Pay close attention to the "WiFi Setup" portion. If you don't do it right, it's going to be a pain!
2. Connect to the center MicroUSB port (marked `USB`, not `PWR IN`). If you connect to the `PWR IN` port, nothing will happen.
3. Be patient! The Raspberry Pi usually takes about 20-30 seconds to boot up. If you don't see the USB volume appear on your machine, give it a few more seconds.
4. Don't run programs directly off of the ChatterSync device. Copy them to the machine's internal memory first. This is the same rule you would follow when using a USB drive.
5. When files are updated, the device will disconnect and reconnect from the machine. This is normal behavior.

## WiFi Setup
**This step is not complex, but it is important that you follow the steps EXACTLY, otherwise you will run into trouble.**
1. Insert the MicroSD card into your computer
2. You should see a drive called `bootfs` or `boot` appear. Open this drive.
3. In the root of the drive, create a file called `wpa_supplicant.conf`
4. Open the file in a text editor and paste the following:
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="YOUR_SSID"
    psk="YOUR_PASSPHRASE"
}
```
Replace `YOUR_SSID` with the name of your WiFi network and `YOUR_PASSPHRASE` with the password for your WiFi network. The 2-letter country code should be that of your country.

5. Insert the MicroSD card into the Raspberry Pi and power it on. It should connect to your WiFi network automatically.

## Usage
Simply connect the device to your CNC machine's USB port and then use an SFTP client to facilitate file transfer (of course, we recommend the free [Chatter VSCode Extension](https://marketplace.visualstudio.com/items?itemName=chatter-dev.chatter-nc-editor)).
Give the device 20-30 seconds to boot.
Please note: When files are updated, the device will pretend to disconnect and reconnect. This is normal behavior. It is *NOT* recommended to run NC file directly off of the device. Instead, copy them to the machine's internal memory before running them. Same rules you would follow when using a USB drive.

## Finding the ChatterSync Device IP

To connect to the ChatterSync device, you will need to know the IP address of the device. There are two ways to do this:

### Option A - Using ChatterSync Discovery Tool (GUI)
Once you've configured your WiFi and powered on your ChatterSync device, you can use our IP discovery tool located at [https://app.chatter.dev/chattersync.php](https://app.chatter.dev/chattersync.php). There you'll see a list of devices sending heartbeats from your public IP. Please make sure that whatever device you are using to run this tool is on the same network as the ChatterSync device.

You can either use this IP to manually add to your list of machines in the VS Code extension, or you can click "View Settings JSON" and then copy and paste the JSON into the settings JSON under `"chatterNcEditor.ftpMachines"`. Be sure to add a comma before the previous machine in the list if you are adding a second machine.

### Option B - Manual Network Scan (Command Line)
One of the silly parts of this setup, given the "headless" nature of the Raspberry Pi device, is finding it on the network. A simple way to find the device on the network is as follows:
* Open PowerShell (NOT Command Prompt)
* Install nmap: `winget install nmap`
* Run the following command to find devices with "chatter" in their name: `nmap -sL 10.0.1.1-255 | Select-String 'chatter'` Replace the ip range with the proper one for your network if needed (eg, 192.168.1.1-255)
* You should see a list of devices with "chatter" in their name. The default hostname of the device is `chattersync`. If the list is blank, your ChatterSync device is likely not on the network.

(Instructions are for Windows, but nmap is available for Mac and Linux as well)

## Compatibility
We are simply emulating a flash drive so in theory, if you can use a flash drive, you can use this. Below is a list of what we and the community have tested. If you test this on a machine not listed below, please let us know (or submit a Pull Request to update this README).

* Compatible with Haas Classic version 13 or later. If you have a big LCD screen, you're good to go. If you have a small LCD screen, go to your "List Program" screen. If you see an option for USB, you're good to go.
* Tested with Fanuc 31i-B. This is a pretty good option if you don't have a dataserver.
* Tested with Hurco machines back to 2018 (Winmax Mill version 10.2.208), should work with older machines as well but this has not been tested.
* Tested on Siemens 840D SL (on a DMG Mori)
* Tested with Bondor tube laser (unknown control version)
* Limited compatability with Syntec controls (tested on 21ma), the automatic mount/unmount does not work properly so the device must be manually unplugged/plugged in when files are changed remotely.
* Currently incompatible with Brother B00 control, causing a freeze on the file IO screen. We plan to test with another type of filesystem to see if this fixes the issue.
* Haas NGC compatibility is questionable. We're doing our best to figure out why, but given that NGC controls all have FTP natively, it's not a huge deal - just save yourself the time and use what is built into the machine.
* Compatible with Okuma machines at least back to 2019, tested on OSP-P300MA and OSP-P300MA-H
* Compatible with YCM machines with Fanuc control at least back to 2014, tested on MXP-200FA
* Not compatible with Datron Neo Series 2, texted on 2022 Next Control

## Customization
The provided image is a "blank slate" that will serve most shops' needs. However, there are a few things you may want to customize.
1. The hostname of the Raspberry Pi is `chattersync`. You can change this by editing the `/etc/hostname` file or by using the `raspi-config` tool. Then name your multiple chattersync devices something like `chattersync1`, `chattersync2`, etc. so they are easy to identify on your network.
2. The default username and password are `chatter` and `chatter`, respectively. You can change these by using the `passwd` command. Changing the password is recommended, [https://passwordsgenerator.net/](https://passwordsgenerator.net/) is a good tool for generating secure random passwords.
3. The default IP address is set to use DHCP. If you want to set a static IP address, you can do so by editing the `/etc/dhcpcd.conf` file.
4. The default size of the USB emulation volume is set to 2GB. If you would like to expand this, you will need to re-image the file located at /usr/share/chattersync.bin. Please note that you will lose all data on the USB emulation volume when you do this, so plan accordingly.
5. By default, the USB volume is "read-only" to the machine. If you change the .env file located in `/usr/local/share/chattersyncdaemon` and set `READ_ONLY=false`, the machine will be able to write to the USB volume. For lazy folks, the command is `sudo nano /usr/local/share/chattersyncdaemon/.env`. This has not been tested extensively, so use at your own risk.

## Troubleshooting
* If the Raspberry Pi does not boot, it may not be getting sufficient power from your machine. Try using a MicroUSB power supply plugged into the `PWR IN` port along with the `USB` port for data transmission.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct IP address and that the Raspberry Pi is connected to your network. Logging into your router and looking for a client named `chattersync` is a good way to check.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct username and password. The default username and password are `chatter` and `chatter`, respectively.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct protocol selected. The Raspberry Pi only supports the `sftp` protocol.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct port selected. The Raspberry Pi uses port 22.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct path selected. The Raspberry Pi uses `/mnt/chatterchattersync/` as the default path.
* If the machine is unable to read the USB, first test it on your PC to make sure you see the volume connected. If you do not see the volume connected, the issue is likely your USB cable. Try a different cable.
* If your PC is able to see the USB volume, but your machine is not able, there may be an issue either with compatability or with the amount of power supplied. Try using a powered USB hub, or attach another power source to the Raspberry Pi on the port marked `PWR IN` (still use the `USB` port for data transmission).

## FAQ
* Does this send my data to Chatter? No.
* Is this ITAR / CMMC compliant? Yes? That's up to you. It's on your network, not ours.
* Why is this free? We're working to build things that help shops. Sometimes that's not feasible to make a product out of, so we just release this stuff for free.
* Can I use this commercially? Yes. You can use this for whatever you want in accordance with the license.
* Can I resell this? Go for it! Just make sure you're in complicance with the Apache 2.0 license included in this repo. Maybe you can resell Chatter Monitoring too :\)
* Can I contribute to this? Yes, please! We'd love to see what you come up with.
* What if I have a problem? Join our [Discord Server](https://discord.com/invite/jjKvU3nuaf) and bug us. If you're a developer, feel free to create an issue or pull request on this repo.

## TODO
* Fine-tune the enclosure models - they are usable but far from perfect.
* Modify daemon to lock out writing of files that are currently open by the machine and pause unmount/mount cycle so that files can safely be run off of the ChatterSync device.
* Test machine write to USB emulation volume.
* Create utility for resizing/customizing volume options.

## Changelog

View the [CHANGELOG.md](CHANGELOG.md) file for a detailed list of changes between versions.

## About Chatter
[Chatter](https://chatter.dev/) is a manufacturing software company based in Roseville, CA, and our mission is to modernize the way that shops use their data. This company was started in a working machine shop, born out of frustration with most software tools for industry being expensive, outdated, and difficult to use. Our goal is to make software that provides real, tangible improvements to the way that shops operate.

Our first product, Chatter Machine Monitoring, is used in shops around the world to provide realtime notifications, remote viewing, and analytics for CNC machines. From there, we have expanded into creating tools for the entire shop. Some of them we release for free because we simply believe they need to exist, and others are in our paid plans.

Twitter/X: [@peteoxenham](https://x.com/peteoxenham)

Instagram: [@chatter.dev](https://instagram.com/chatter.dev)
