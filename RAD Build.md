# Remote Access Device (RAD) – Build Guide Overview

This repository documents the design, hardware requirements, and software configuration for a **Remote Access Device (RAD)** built on the **LattePanda Delta V3** platform.

The system is designed for:
- Remote access over cellular networks
- Secure VPN connectivity
- Wireless monitoring and analysis
- GPS-based location awareness

This project is intended for **authorized security research, lab environments, and educational use**.

---

## Disclaimer

> This project is provided for educational and authorized security testing purposes only.  
> The author assumes no responsibility for misuse, unauthorized deployment, or violations of law or policy.

---

## Hardware Requirements

| # | Component |
|--|----------|
| 1 | LattePanda Delta V3 |
| 2 | GlobalSat BU-353S4 USB GPS Receiver |
| 3 | Alfa AC1900 Long-Range USB Wireless Adapter |
| 4 | USB Drive (32GB recommended) |
| 5 | USB-to-Ethernet Adapter |
| 6 | GL.iNet GT750 Travel Router (SIM required – AT&T or T-Mobile) |
| 7 | SIM7600G-H-M.2 4G Communication Module |
| 8 | HDMI Video Source (for initial setup) |

---

## Software Stack

- Kali Linux
- Tailscale VPN
- Kismet

---

## Installation Guide

### Step 1: Download Kali Linux

1. Insert and format a USB drive.
2. Navigate to: https://www.kali.org
3. Download the **Kali Linux Installer (64-bit)**.
4. Copy the `.iso` file to the USB drive.
5. Safely eject the USB drive.

---

### Step 2: Install Kali Linux on LattePanda Delta V3

1. Insert the USB drive into the LattePanda Delta V3 and power it on.
2. Connect a keyboard, mouse, and monitor.
3. Select **Graphical Install** from the Kali boot menu.
4. Proceed until **Partition Disks**.
5. Select **Use Entire Disk**.
6. Complete installation, remove USB, and reboot.

---

### Step 3: Install Core Kali Toolsets

Update the system:

`sudo apt update && sudo apt upgrade -y`

Install the full Kali toolset:

`sudo apt install kali-linux-everything`

This process may take several hours.

Connect the following hardware once complete:

USB-to-Ethernet adapter

Alfa AC1900 wireless adapter

GPS receiver

---

### Step 4: Install Alfa AC1900 Drivers

Update system:

`sudo apt update && sudo apt upgrade -y`

Install dependencies:

`sudo apt install build-essential dkms`

Identify chipset:

`lsusb`

Clone and install driver:

`git clone https://github.com/aircrack-ng/rtl8812au.git`

`cd rtl8812au`

`make`

`sudo make install`

Verify adapter:

`iwconfig`

---

### Step 5: Install and Configure GPS Receiver

Install GPS tools:

`sudo apt install gpsd gpsd-clients python-gps`

Identify GPS device:

`dmesg | grep ttyUSB`

Edit GPSD configuration:

`sudo nano /etc/default/gpsd`

Set values:

START_DAEMON="true"

GPSD_OPTIONS=""

DEVICES="/dev/ttyUSB0"

USB_AUTO="true"


Validate GPS lock:

`cgps`

---

### Step 6: Install Tailscale VPN

Install Tailscale:

`curl -fsSL https://tailscale.com/install.sh | sh`

`sudo apt install tailscale`

Enable and start service:

`sudo systemctl enable tailscaled`

`sudo systemctl start tailscaled`

Authenticate device:

`sudo tailscale up`

Follow the provided URL to authorize the device.

---

### Step 7: Install and Configure Kismet

Install Kismet:

`sudo apt install kismet`

Switch to root:

`sudo su`

Edit configuration:

`cd /etc/kismet`

`nano kismet.conf`

Enable GPS integration:

`gps=gpsd:host=localhost,port=2947`

Start Kismet:

`kismet`

Access web interface:

`http://localhost:2501`

---

### Operational Notes

Supports cellular-based remote access

Encrypted mesh VPN connectivity via Tailscale

Wireless monitoring with GPS correlation

Suitable for red team labs, research deployments, and mobile test platforms

---

### Future Enhancements

Encrypted storage

Persistence mechanisms

Remote telemetry logging

Power optimization

Custom C2 overlays

---

### License

This project is released for educational and research purposes.
Refer to individual tool licenses for third-party software.
