# C++ Library for TEO: Ephemeral Ownership for IoT Devices to Provide Granular Data Control

- [C++ Library for TEO: Ephemeral Ownership for IoT Devices to Provide Granular Data Control](#c-library-for-teo-ephemeral-ownership-for-iot-devices-to-provide-granular-data-control)
  - [Dependency](#dependency)
  - [Setup](#setup)
  - [Build](#build)
  - [CMake configuration options](#cmake-configuration-options)
  - [Command Line Test Apps](#command-line-test-apps)
    - [Basic Functionality](#basic-functionality)
    - [Manual Exploration](#manual-exploration)
  - [Case Studies](#case-studies)
    - [Motion Camera](#motion-camera)
    - [Mycroft AI Speaker Assistant](#mycroft-ai-speaker-assistant)
    - [Smart Doorlock](#smart-doorlock)
  - [Reference](#reference)


## Dependency

The following dependencies are included in the [setup script](#setup). We are enumerating them just for your reference. No need to take any action if you plan to use our provided setup script.

- `CryptoPP`: Need this library for Shamir secret sharing.
Install CryptoPP with CMake: https://www.cryptopp.com/wiki/CMake
If you install it manually, don't forget to run `make install` before
linking the library in this project's CMake.
- `libdecaf`: This library provides full implementation of Ed448 and support inverse lookup of elligator, i.e. translating a hash to an EC point and vice versa. https://sourceforge.net/p/ed448goldilocks/code/ci/master/tree/
- `libsodium`: Better crypto library than CryptoPP. https://github.com/jedisct1/libsodium


## Setup

Instructions on how to set up on a fresh Ubuntu machine.

- Run `./bin/setup.sh`
  - Must use **GCC-9**, as one of the dependency (json library) doesn't play well with GCC-10.

- **[Additional step for storage server]**
Run `./bin/setup_storage.sh`
  - You need to install additional dependencies if you want to build storage module (hence deploy the storage server on targeted platforms).

## Build

- **[Optional]** Run `./bin/compile_flatbuffers_models.sh --cpp -o include/teo/` to generate the flatbuffer files for message format. (This step is included in the [setup](#setup) script.)
- Run `cmake -B build -S .` to generate a buildsystem and then run the actual build command `cmake --build build`. 

## CMake configuration options

Pass these options to CMake configuration command, e.g.

| CMake option | Values | Description |
| ------------ | ------ | ----------- |
| TEO_EXTENDED_TESTS | ON / ***OFF*** | Run additional tests (Please leave off, deprecated) |
| TEO_STANDALONE_APP | ***ON*** / OFF | Build standard Linux app (instead of Android native libraries) |
| TEO_STORAGE_MODULE | ***ON*** / OFF | Build storage module for third-party storage server |
| TEO_DEMO_APPS | ***ON*** / OFF | Build apps for demonstration |
| TEO_BLUETOOTH_BEACON | ON / ***OFF*** | Enable bluetooth beacons for proximity keep-alive |
| JSON_BuildTests | ***ON*** / OFF | [Third-party] JSON library unit tests (leave on if you care or want to make sure library works) |

## Command Line Test Apps

### Basic Functionality

Run the simple unit test:
```bash
# terminal 1
./build/apps/storage

# terminal 2: collect storage server information from above
./build/apps/app <storage-ip> <storage-port>
```
This standalone test app demonstrates some basic TEO functinalities and crypto primitives. You can check out its implementation at 

### Manual Exploration

It is important that you start the following terminals/sessions/programs in the correct sequence. However, you do have some flexibility for exploration once the system is up and running (once user becomes an ephemeral owner).

1. Start the storage server.
   ```bash
   # terminal 1
   ./build/apps/storage
   # Collect storage server's info
   teo-storage> info 
   ```
2. Start the device program.
   ```bash
   # terminal 2
   ./build/apps/device <storage-ip> <storage-port>
   # Collect device's info for **admin**
   teo-device> info admin
   ```
3. Start the admin program.
    ```bash
    # terminal 3
    ./build/apps/admin <storage-ip> <storage-port> <device_pubkey> <device_secret>
    # Attempt to initialize the targeted device
    teo-admin> initDevice
    ```

## Case Studies

TEO integration with existing applications. They are designed for the Raspberry Pi 4 platform.

### Motion Camera

See `cases/motion`.

### Mycroft AI Speaker Assistant

See `cases/mycroft`.

### Smart Doorlock

See `cases/doorlock`.


## Reference

Learn CMake basics: https://cliutils.gitlab.io/modern-cmake/

Reference repository: https://gitlab.com/CLIUtils/modern-cmake/tree/master/examples/extended-project

