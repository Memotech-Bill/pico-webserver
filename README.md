# pico-webserver

This fork has been revised to build alongside pico-sdk (includes tinyusb) and
pico-extras (includes lwip), rather than importing sub-modules.

## Raspberry Pi Build Instructions

If the Pico development environment has not been installed, first do that:

    wget https://raw.githubusercontent.com/raspberrypi/pico-setup/master/pico_setup.sh
    chmod +x pico_setup.sh
    ./pico_setup.sh

Then build the webserver:

    cd pico
    git clone https://github.com/Memotech-Bill/pico-webserver.git
    cd pico-webserver
    mkdir build
    cd build
    cmake ..
    make

Copy the resulting pico_webserver.uf2 file onto the Pico in the usual fashion.

Establishing the USB Network connection seems to be unreliable. This is under investigation.

# Original README

Webserver example that came with TinyUSB slightly modified to run on a Raspberry Pi Pico.
Lets the Pico pretend to be a USB Ethernet device. Runs a webinterface at http://192.168.7.1/

## Build dependencies

On Debian:

```
sudo apt install git build-essential cmake gcc-arm-none-eabi
```

Your Linux distribution does need to provide a recent CMake (3.13+).
If not, compile [CMake from source](https://cmake.org/download/#latest) first.

## Build instructions

```
git clone --depth 1 https://github.com/maxnet/pico-webserver
cd pico-webserver
git submodule update --init
mkdir -p build
cd build
cmake ..
make
```

Copy the resulting pico_webserver.uf2 file to the Pico mass storage device manually.
Webserver will be available at http://192.168.7.1/

Content it is serving is in /fs
If you change any files there, run ./regen-fsdata.sh

By default it shows a webpage that led you toggle the Pico's led, and allows you to switch to BOOTSEL mode.
