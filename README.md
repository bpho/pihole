# Raspberry Pi Zero W Pi-hole Setup Plan

This repo tracks the setup plan and progress for running Pi-hole on a Raspberry Pi Zero W Basic.

## Goal

Set up the Raspberry Pi Zero W as a lightweight network-wide DNS sinkhole using Pi-hole, then point the home network at it for ad blocking and DNS visibility.

## Hardware

- Raspberry Pi Zero W Basic
- microSD card, 8 GB or larger
- USB power supply
- Computer for flashing Raspberry Pi OS
- Home Wi-Fi network credentials

## Setup Plan

### 1. Prepare the microSD card

- Install Raspberry Pi Imager on the computer.
- Flash Raspberry Pi OS Lite, 32-bit, to the microSD card.
- Configure the image before writing:
  - Set hostname, for example `pihole`.
  - Enable SSH.
  - Set username and password.
  - Add home Wi-Fi SSID and password.
  - Set locale, timezone, and keyboard layout.

### 2. Boot and connect

- Insert the microSD card into the Raspberry Pi Zero W.
- Power on the Raspberry Pi.
- Wait a few minutes for first boot.
- Find the device on the network from the router client list or with:

```bash
ping pihole.local
```

- SSH into the Pi:

```bash
ssh <username>@pihole.local
```

### 3. Update the system

```bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
```

Reconnect over SSH after the reboot.

### 4. Set a stable IP address

Preferred option: reserve a static DHCP lease for the Pi in the router using its MAC address.

Fallback option: configure a static IP on the Pi if router DHCP reservation is not available.

Record the chosen IP address here:

```text
Pi-hole IP: TBD
```

### 5. Install Pi-hole

Run the official installer:

```bash
curl -sSL https://install.pi-hole.net | bash
```

During setup:

- Select the active Wi-Fi interface.
- Confirm the static IP address.
- Choose an upstream DNS provider.
- Enable the web admin interface.
- Enable query logging unless there is a privacy reason not to.

### 6. Save admin details

After installation, record the admin URL:

```text
Admin URL: http://<Pi-hole IP>/admin
```

If needed, reset the admin password:

```bash
pihole setpassword
```

### 7. Point the network at Pi-hole

Preferred option: set the router DHCP DNS server to the Pi-hole IP address.

Fallback option: manually set DNS on individual devices to the Pi-hole IP address.

Do not remove the old DNS settings until Pi-hole has been tested.

### 8. Validate the setup

- Confirm the Pi-hole admin page loads.
- Confirm clients appear in the Pi-hole dashboard.
- Confirm DNS resolution works:

```bash
nslookup google.com <Pi-hole IP>
```

- Confirm ad blocking works by checking the query log while browsing.

### 9. Maintenance

Update Pi-hole periodically:

```bash
pihole -up
```

Check system health:

```bash
uptime
df -h
pihole status
```

## Progress Log

Use `progress.md` for notes, decisions, IP addresses, router settings, and troubleshooting steps as the setup moves forward.
