# CAN-to-USB-Linux-setting-and-running
# IXXAT USB to CAN v2 Compact Linux Setup Guide

## Table of Contents
- [Overview](#overview)
- [Supported Devices](#supported-devices)
- [Prerequisites](#prerequisites)
- [Installation Guide](#installation-guide)
- [Configuration](#configuration)
- [Usage Examples](#usage-examples)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)

## Overview

This repository provides comprehensive instructions for setting up and configuring the IXXAT USB-to-CAN Linux driver. The driver is specifically designed for real-time operating systems and enables seamless communication between USB-to-CAN devices and Linux systems.

## Supported Devices

The driver provides support for the following IXXAT devices:
- USB-to-CAN V2 compact
- USB-to-CAN V2 embedded
- USB-to-CAN V2 professional
- USB-to-CAN V2 automotive
- USB-to-CAN V2 plugin
- USB-to-CAN V2 FD compact
- USB-to-CAN V2 FD professional
- USB-to-CAN V2 FD automotive
- USB-to-CAN V2 FD MiniPCIe
- USB-to-CAR
- CANIDM101

## Prerequisites

### System Requirements
- Linux Operating System
- Kernel Headers
- Build Essential Tools

### Required Packages
```bash
# Install kernel headers
sudo apt install linux-headers-$(uname -r)

# Install build dependencies
sudo apt install flex
sudo apt install bison build-essential
```

## Installation Guide

### 1. Driver Download
1. Visit the [HMS Networks Support](https://www.hms-networks.com/support/general-downloads) website
2. Navigate to: Linux Drivers → socketCAN driver for Linux
3. Download the socketcan-linux.gz package
4. Extract the downloaded package

### 2. Kernel Header Configuration
```bash
# Navigate to kernel headers directory
cd /usr/src/linux-headers-$(uname -r)

# Configure headers
sudo make oldconfig
```

### 3. Driver Build and Installation
```bash
# Navigate to driver directory
cd path/to/ix_usb_can_2.0.529REL

# Clean previous builds
make clean

# Build the driver
make

# Install the driver
sudo make install

# Load the driver module
sudo modprobe ix_usb_can
```

### 4. Verification
```bash
# Verify module loading
lsmod | grep ix_usb_can

# Check CAN interface
ip link
```

## Configuration

### Basic Setup
```bash
# Install CAN utilities
sudo apt install can-utils

# Configure CAN interface (default bitrate: 250000)
sudo ip link set can0 up type can bitrate 250000
```

### Auto-restart Configuration
```bash
# Configure automatic restart after bus-off condition (500ms)
ip link set can0 type can restart-ms 500
```

## Usage Examples

### Basic CAN Communication
```bash
# Monitor all CAN traffic
candump can0

# Stop CAN interface
ip link set can0 down

# Start CAN interface
ip link set can0 up
```

### Advanced Usage

#### Message Filtering
```bash
# Filter by specific CAN ID (Example: ID 1AA with mask 1A3)
candump can0,1A31AA
```

#### Data Logging
```bash
# Log with timestamps
candump -t z can0

# Save to log file (generates: candump-<date>_XXXXX.log)
candump -l can0
```

## Troubleshooting

### Bus-off Condition
A bus-off condition occurs when:
- The CAN controller's transmit error counter exceeds 255
- Communication is halted
- Recovery requires detection of 128 occurrences of 11 consecutive recessive bits

### Common Issues and Solutions

#### Device Not Detected
1. Check USB connection
2. Verify device recognition:
```bash
lsusb
ip link
```

#### Driver Loading Issues
Check system logs:
```bash
sudo dmesg | grep -i can
```

## Additional Resources

### Documentation
- [Official HMS Networks Documentation](https://www.hms-networks.com/support)
- [SocketCAN Documentation](https://www.kernel.org/doc/Documentation/networking/can.txt)

### Support
For additional support:
- Visit the [HMS Networks Support Portal](https://www.hms-networks.com/support)
- Check system logs using `dmesg`
- Review kernel documentation for CAN subsystem

## License
[Add your license information here]

## Contributing
[Add contribution guidelines if applicable]
