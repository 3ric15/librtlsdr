[![librtlsdr version](https://img.shields.io/github/tag/librtlsdr/librtlsdr.svg?style=flat&label=librtlsdr)](https://github.com/librtlsdr/librtlsdr/releases)
[![GPLv2 License](http://img.shields.io/badge/license-GPLv2-brightgreen.svg)](https://tldrlegal.com/license/gnu-general-public-license-v2)

# Description

Modified version of librtlsdr for rtl_tcp to support dithering and selecting RTL-SDRs by serial numbers. Then Osmocom RTL-SDR source blocks within GNURadio Companion can be assigned IP addresses for I/Q data. 

Running ```rtl_tcp -d :1 -N 0``` will disable dithering and choose the RTL-SDR with serial number 1.

## Example

Stream data locally from two RTL-SDRs, with dithering disabled.
```
rtl_tcp -a 127.0.0.1 -p 1232 -f 1000000 -s 2400000 -g 1 -d :1 -N 0 & 
rtl_tcp -a 127.0.0.1 -p 1234 -f 1000000 -s 2400000 -g 1 -d :2 -N 0
```

In GRC, type the following device arguments into the first osmocom RTL-SDR Source block:
```
rtl_tcp=127.0.0.1:1232
```

and likewise for the second source block:
```
rtl_tcp=127.0.0.1:1234
```

# Build / Install (on debian/ubuntu)

## prerequisites

```
sudo apt-get install libusb-dev libusb-1.0-0-dev build-essential cmake git
```

## Purge old RTL-SDR and librtlsdr install

NOTE: this could break other RTL-SDR programs if they were relying on a specific driver branch

```
sudo apt purge librtlsdr*
sudo rm -rvf /usr/lib/librtlsdr* /usr/include/rtl-sdr* /usr/local/lib/librtlsdr* /usr/local/include/rtl-sdr*
```

## Install librtlsdr to system

```
git clone https://github.com/ehelgesen1/librtlsdr
cd librtlsdr
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo ldconfig 
```
