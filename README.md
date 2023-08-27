# ChatterSync - Network File Transfer of USB for Un-Networked CNC Machines

## Overview
This project is a simple file transfer system to improve the program transfer capabilites of CNC machines that do not have networking capabilities (pre-NGC Haas, for example). We've developed a device that that connects to the machine's USB port and behaves as a flash drive, but also is accessible over the WiFi network for file transfer. This allows the machine to be connected to the network without any modifications to the machine itself.

We've adopted a "DIY" model for this. Existing systems to do similar things are prohibitavely expensive, so we provide you all the resources to get it going yourself for less than $30 per machine.

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
4. Configure your WiFi network by editing the `wpa_supplicant.conf` file on the MicroSD card. Instructions for this can be found [here](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md).
5. Eject the MicroSD card and insert it into the Raspberry Pi Zero W. Connect the Raspberry Pi to your CNC machine using the MicroUSB to USB-A cable.
6. Note the IP address of the Raspberry Pi. You can find this by logging into your router and looking at the list of connected devices. Alternatively, you can use a tool like [Advanced IP Scanner](https://www.advanced-ip-scanner.com/) to scan your network for the Raspberry Pi. The hostname of the Raspberry Pi is `chattersync`.
7. Open the Visual Studio Code with the [Chatter NC Editor Extension](https://marketplace.visualstudio.com/items?itemName=chatter-dev.chatter-nc-editor) installed and select "Add Machine".
8. Follow the prompts, using the IP address of the Raspberry Pi as the "Machine Address". The "Machine Name" can be whatever you want to call it. For "protocol", select "sftp" (this will default to port 22). This is the most secure option. For "username", enter `chatter`, and for "password", enter `chatter`. These are the default credentials for this image.
9. Once you have added the machine, click the settings cogwheel and change the property `basePath` to `/mnt/chattersync`. This will allow you to access the USB emulation volume over the network.
10. You should now be able to connect to the Raspberry Pi and transfer files to your CNC machine.

## Usage
Simply connect the device to your CNC machine's USB port and then use an SFTP client to facilitate file transfer (of course, we recommend the free [Chatter VSCode Extension](https://marketplace.visualstudio.com/items?itemName=chatter-dev.chatter-nc-editor)).
Please note: When files are updated, the device will pretend to disconnect and reconnect. This is normal behavior. It is *NOT* recommended to run NC file directly off of the device. Instead, copy them to the machine's internal memory before running them. Same rules you would follow when using a USB drive.

## Customization
The provided image is a "blank slate" that will serve most shops' needs. However, there are a few things you may want to customize.
1. The hostname of the Raspberry Pi is `chattersync`. You can change this by editing the `/etc/hostname` file or by using the `raspi-config` tool.
2. The default username and password are `chatter` and `chatter`, respectively. You can change these by using the `passwd` command.
3. The default IP address is set to use DHCP. If you want to set a static IP address, you can do so by editing the `/etc/dhcpcd.conf` file.
4. The default size of the USB emulation volume is set to 2GB. If you would like to expand this, you will need to re-image the file located at /usr/share/chattersync.bin. Please note that you will lose all data on the USB emulation volume when you do this, so plan accordingly.
5. By default, the USB volume is "read-only" to the machine. If you change the .env file located in `/usr/local/share/chattersyncdaemon` and set `READ_ONLY=false`, the machine will be able to write to the USB volume. For lazy folks, the command is `sudo nano /usr/local/share/chattersyncdaemon/.env`. This has not been tested extensively, so use at your own risk.

## Troubleshooting
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct IP address and that the Raspberry Pi is connected to your network.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct username and password. The default username and password are `chatter` and `chatter`, respectively.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct protocol selected. The Raspberry Pi only supports the `sftp` protocol.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct port selected. The Raspberry Pi uses port 22.
* If you are unable to connect to the Raspberry Pi, make sure that you have the correct path selected. The Raspberry Pi uses `/home/chatter` as the default path.

## FAQ
* Does this send my data to Chatter? No.
* Is this ITAR / CMMC compliant? Yes. It's on your network, not ours.
* Why is this free? We're working to build things that help shops. Sometimes that's not feasible to make a product out of, so we just release this stuff for free.
* Can I use this commercially? Yes. You can use this for whatever you want.
* Can I resell this? Go for it! Maybe you can resell Chatter Monitoring too :\)
* Can I contribute to this? Yes, please! We'd love to see what you come up with.
* What if I have a problem? Join our [Discord Server](https://discord.com/invite/jjKvU3nuaf) and bug us. If you're a developer, feel free to create an issue or pull request on this repo.

## TODO
* Fine-tune the enclosure models - they are usable but far from perfect.
* Test machine write to USB emulation volume.
* Create utility for resizing/customizing volume options

## About Chatter
[Chatter](https://chatter.dev/) is a manufacturing software company based in Roseville, CA, and our mission is to modernize the way that shops use their data. This company was started in a working machine shop, born out of frustration with most software tools for industry being expensive, outdated, and difficult to use. Our goal is to make software that provides real, tangible improvements to the way that shops operate.

Our first product, Chatter Machine Monitoring, is used in shops around the world to provide realtime notifications, remote viewing, and analytics for CNC machines. From there, we have expanded into creating tools for the entire shop. Some of them we release for free because we simply believe they need to exist, and others are in our paid plans.

Twitter/X: [@peteoxenham](https://x.com/peteoxenham)

Instagram: [@chatter.dev](https://instagram.com/chatter.dev)
