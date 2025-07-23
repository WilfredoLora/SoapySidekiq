# Soapy SDR module for Epiq Solutions Sidekiq

## Dependencies
* LIBTIRPC - sudo apt-get install libtirpc-dev or use the one that comes with the SideKiq SDK
* SoapySDR - https://github.com/pothosware/SoapySDR/wiki
* sidekiq API - https://epiqsolutions.com/rf-transceiver/sidekiq/

## Documentation

### Prerequisites: Prepare Sidekiq SDK Files

Go to your Sidekiq SDK directory.
Example:

This is an example of where to find your SideKiq SDK folder, This instance 4.18.1-13 was used.
```
cd /home/username/sidekiq_sdk_v4.18.1-13-g58be05ffb/
```

Copy header files to system include directory:
```
cd sidekiq_core/inc
sudo cp *.h /usr/local/include/
```
Copy library file to system library directory:
Go back to the main SDK directory:
```
cd ../..
```

Enter the library directory:
```
cd lib
```

Copy the correct library for your system.
(For most 64-bit systems, use libsidekiq__x86_64.gcc.a)

```
sudo cp libsidekiq__x86_64.gcc.a /usr/local/lib/
```

## Install SoapySidekiq
#### Clone and build SoapySidekiq:

```
git clone https://github.com/WilfredoLora/SoapySidekiq.git
cd SoapySidekiq
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig
```
## Verify SoapySidekiq Installation
#### Check that SoapySDR detects the Sidekiq module:

```
SoapySDRUtil --info
```

You should see output similar to:
```
Module found: /usr/local/lib/SoapySDR/modules0.8-3/libSidekiqSupport.so
```
Under Available factories, sidekiq should be listed.

Additional Information:
* https://github.com/pothosware/SoapySidekiq/wiki

## Licensing information

* https://github.com/pothosware/SoapySidekiq/blob/master/LICENSE
