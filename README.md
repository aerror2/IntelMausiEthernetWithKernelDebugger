## Fork Intel Mausi Network Driver by RehabMan

I added the implementation of IOKernelDebugger with IOKDP

So anyone use these NIC  can do the kernel debugging between  two real machine. 

Details about how to make this happen. please refer https://www.tonymacx86.com/threads/discussion-about-how-to-make-nvidia-web-driver-works-in-macos-10-14-mojave.265813/#post-1863247




### How to Install:
 copy the /Volume/EFI/EFI/CLOVER/kexts/Other


### How to debug:

Install the KDK from developer.apple.com

Please follow the KDK readme.html. 

lldb will search /Library/Developer/KDKs/ as well as any local directories indexed by Spotlight.

You invoke lldb by the simple command:

lldb

Alternatively, you can specify the kernel file on the command line.

lldb /Library/Developer/KDKs/<KDK Version>/System/Library/Kernels/kernel

To attach, you either use kdp-remote for a live connection, or file -c for a coredump.

kdp-remote {name_or_ip_address}
file -c {path_to_coredump}

Two machine debugging is not supported via USB Ethernet.
Two machine debugging is not supported via wireless.

Network Debugging

The default setting for two machine debugging is as follows:

sudo nvram boot-args="debug=0x146"
sudo reboot



You can raise the debug event after network is initialized using the combo key :

shift + ctrl + option + comand + esc




### Original README follows:

# IntelMausiEthernet
OS X driver for Intel onboard LAN

A few days before Christmas I started my latest project, a new driver for recent Intel onboard LAN controllers. My intention was not to replace hnak's AppleIntelE1000e.kext completely but to deliver best performance and stability on recent hardware. That's why I dropped support for a number of older NICs. Currently the driver supports:
 
- 5 Series
  - 82578LM
  - 82578LC
  - 82578DM
  - 82578DC
- 6 and 7 Series
  - 82579LM
  - 82579V
- 8 and 9 Series
  - I217LM
  - I217V
  - I218LM
  - I218V
  - I218LM2
  - I218V2
  - I218LM3
- 100 Series
  - I219V
  - I219LM
  - I219V2
  - I219LM2
  - I219LM3

Key Features of the Driver
- Support for multisegment packets relieving the network stack of unnecessary copy operations when assembling packets for transmission.
- No-copy receive and transmit. Only small packets are copied on reception because creating a copy is more efficient than allocating a new buffer.
- TCP, UDP and IPv4 checksum offload (receive and transmit).
- Support for TCP/IPv6 and UDP/IPv6 checksum offload.
- Makes use of the chip's TCP Segmentation Offload (TSO) feature with IPv4 and IPv6 in order to reduce CPU load while sending large amounts of data.
- Fully optimized for Yosemite and Mavericks (64bit architecture) but should work with Mountain Lion and Lion too, provided you build from source with the 10.8 or 10.7 SDK.
- Support for Energy Efficient Ethernet (EEE).
- VLAN support is implemented but untested as I have no need for it.
- The driver is published under GPLv2.
