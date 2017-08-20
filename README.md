# Raspberry Pi Ad-Hoc Network

## What you will need:

More than one Raspberry Pi (a model 2 or 3 will do).

Each with a microSD card with the latest version of Raspbian installed.

Please note that if you are using a Raspberry Pi Model 2 you will need to get a USB wireless adapter, since this project covers setting up wlan0 as our access point to the internet.

## Setting up the Ad-Hoc network

When your Raspberry pi boots up, you should be taken to the GUI by default.  If not, log in and type into the command prompt:

```bash
startx
```

Upon entering the GUI, open the menu and under "Accessories" and open the terminal.  When the terminal window opens, you should find a command prompt much similar to the one that you saw when you first started up your Raspberry Pi.

Now, before we delve into editing the interfaces file, we need to make a backup of it so that if, for any reason, you need your Raspberry Pi to revert it's network status, you can just delete the new file rename the old network file and you are fine:

```bash
cp /etc/network/interfaces /etc/network/interfacesbackup
```

Now with the backup created, we can now go ahead and edit the file:

```bash
nano /etc/network/interfaces
```

Replace all text in the file with this text here:

```bash
# interfaces(5) file used by ifup(8) and ifdown(8)

# Please note that this file is written to be used with dhcpcd
# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback

iface eth0 inet dhcp

auto wlan0
iface wlan0 inet static
  address 192.168.1.42
  netmask 255.255.255.0
  wireless-channel 1
  wireless-essid MyCoolMeshNet
  wireless-mode ad-hoc
  ```
  
Replace the IP address of your Raspberry Pi and the SSID of your network to whatever you had in mind, and first look up your wireless channels you can use, because they differ from country to country.

Next, we need to disable dhcpcd, because it will try to give you another IP address:

```bash
systemctl stop dhcpcd.service
```

Do the same steps on all the other Raspberry Pis, keeping the SSID name the same as your chosen one, and changing the last digits in the IP address to something that isn't taken by the other Raspberry Pis.

Now reboot by entering:

```bash
reboot
```

## Testing

When you have more than one Raspberry Pi set up to communicate in the Ad-Hoc network, you should test it now.

In the command prompt, type:

```bash
ping NodeIPAdress
```
Replace NodeIPAddress with the IP address of your other Raspberry Pi that we set up as well.
