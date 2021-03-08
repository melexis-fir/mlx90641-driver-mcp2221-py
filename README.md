# MLX90641 driver for MCP2221 USB I2C hub

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/melexis-fir/mlx90641-driver-mcp2221-py?label=github-latest-release-tag)](https://github.com/melexis-fir/mlx90641-driver-mcp2221-py/releases) [![GitHub Workflow Status](https://github.com/melexis-fir/mlx90641-driver-mcp2221-py/workflows/build-test-publish/badge.svg)](https://github.com/melexis-fir/mlx90641-driver-mcp2221-py/actions?query=event%3Arelease) ![Lines of code](https://img.shields.io/tokei/lines/github/melexis-fir/mlx90641-driver-mcp2221-py)  

[![PyPI](https://img.shields.io/pypi/v/mlx90641-driver-mcp2221)](https://pypi.org/project/mlx90641-driver-mcp2221) ![PyPI - Python Version](https://img.shields.io/pypi/pyversions/mlx90641-driver-mcp2221) ![PyPI - License](https://img.shields.io/pypi/l/mlx90641-driver-mcp2221)  

![platform](https://img.shields.io/badge/platform-Win10%20PC%20%7C%20linux%20PC%20%7C%20rasberry%20pi%204%20%7C%20Jetson%20Nano%20%7C%20beagle%20bone-lightgrey)  

MLX90641 is a thermal camera (16x12 pixels) using Far InfraRed radiation from objects to measure the object temperature.  
https://www.melexis.com/mlx90641  
The python package "[mlx90641-driver](https://github.com/melexis-fir/mlx90641-driver-py)" driver interfaces the MLX90641 and aims to facilitate rapid prototyping.

This package provide the I2C low level routines.
It uses the I2C hub from MCP2221 chip which is connected via the USB cable to the computer.  
https://www.microchip.com/wwwproducts/en/mcp2221  
https://www.adafruit.com/product/4471  

## Getting started

### Installation


```bash
pip install mlx90641-driver-mcp2221
```

https://pypi.org/project/mlx90641-driver-mcp2221  
https://pypistats.org/packages/mlx90641-driver-mcp2221

#### Extra installation for linux based OS.

1. install udev, libusb and libhidapi:
```sh
sudo apt update
sudo apt-get install libudev-dev libusb-1.0-0-dev libhidapi-dev
```

2. Configure such that non-root users have access.

Place a file in `/etc/udev/rules.d/20-microchip.rules`:

```cfg
KERNEL=="hidraw*", ATTRS{idVendor}=="04d8", MODE="0666"
```

You might use this command line to create that file:
```sh
echo 'KERNEL=="hidraw*", ATTRS{idVendor}=="04d8", MODE="0666"' | sudo tee /etc/udev/rules.d/20-microchip.rules >/dev/null
```

__Note:__
Make sure to (re-)plug the MCP2221 after this file is written!


### Running the driver demo

* Connect the MLX90641 to the MCP2221 I2C port
* Connect the MCP2221 to your PC with the USB cable.  
* Open a terminal and run following command:  

```bash
mlx90641-dump-mcp2221 mcp://mcp:2221/0
```

This program takes 1 optional argument.

```bash
mlx90641-dump-mcp2221 <communication-port>
```

Note: this dump command is not yet available!
