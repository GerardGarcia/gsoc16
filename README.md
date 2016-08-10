# GSoC 2016: Work Product 

### QEMU project: AF_VSOCK packet capture in Linux and Wireshark

This repository contains the work done for the Google Summer of Code 2016 program with the QEMU organization. 

Each folder contains the patches sent to each of the projects. 

### Repository overview

* linux/vsockmon: Vsockmon virtual network device. As of 10-08-2016 it is still not merged in part because it was necessary to wait until the virtio-vsock transport was merged (01-08-2016). 

    Related links: 
    * RFC: http://marc.info/?l=linux-netdev&m=146445657415137&w=2
    * RFC v2: http://marc.info/?l=linux-netdev&m=146661193816961&w=2
    * PATCH: http://marc.info/?l=linux-netdev&m=147067289819024&w=2

* wireshark: Wireshark dissector for the vsockmon virtual network device, merged.

    Related links:
    * REVIEW: https://code.wireshark.org/review/#/c/16308/
    * BUG: https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=12623#c0

* tcpdump: Tcpdump printer for the vsockmon virtual network device. As of 10-08-2016 still not merged as we are waiting to get vsockmon merged to get a DLT_/LINKTYPE_ identifier (http://www.tcpdump.org/linktypes.html).

    Related links:
    * RFC: http://lists.sandelman.ca/pipermail/tcpdump-workers/2016-June/000546.html
    * Identifier request: http://lists.sandelman.ca/pipermail/tcpdump-workers/2016-July/000592.html

* linux/virtio-vsock: Several patches related to the virtio transport. As of 10-08-2016 still not merged..

    Related links:
    * VSOCK-remove-more-space-available-check-filling-TX-vq: http://marc.info/?l=linux-netdev&m=147041487402313&w=2
    * Add-timer-to-handle-OOM-situations: http://marc.info/?l=linux-netdev&m=147031989408785&w=2
    * Fix-unbound-rx-buffer: http://marc.info/?l=linux-netdev&m=147031984808760&w=2

### Project proposal

http://qemu-project.org/Google_Summer_of_Code_2016#AF_VSOCK_packet_capture_in_Linux_and_Wireshark 

### Project summary

QEMU is an open source machine emulator and virtualizer. As a virtualizer, it achieves nearly native performance using the Kernel-based Virtual Machine (KVM) hypervisor.

Zero-configuration communication between the hypervisor and its guests can be achieved using the virtio-serial device. The virtio-serial device sits on top of the VirtIO API, which allows the para-virtualization of devices in the guest system independently of the hypervisor.

Virtio-serial has several limitations though. For example, it does not allow multiple connections to the same port, the number of ports is quite limited and it is implemented as a character device (which are not usually used as communication mechanisms). To overcome this limitations the driver virtio-vsock is being developed.

The virtio-vsock device supports the POSIX Sockets API, which is more familiar to developers since it is the mechanism usually used for interprocess communication. The use of sockets have several advantages: N:1 communication, block and stream protocols, API widely known... Furthermore, programs that already use sockets can easily transition to use the virtio-vsock device without major changes in their code.

However, the traffic sent through virtio-vsock is hidden to the outside world, as it is internally managed by the hypervisor and the driver. And being able to snoop this traffic is very important when debugging.

The goal of this project is to expose the traffic exchanged through the virtio-vsock socket interface so that programs like Wireshark or tcpdump can capture it. This will help developers wanting to make use of the virtio-vsock device and will contribute to its adoption by the community.

To achieve this, it would be necessary to implement a device driver that exposes the traffic and a Wireshark dissector to parse it.

### Development notes and related work:

* http://wiki.gvandellos.cat/GSOC16
* http://wiki.gvandellos.cat/GSOC16/virtio-vsock
* http://wiki.gvandellos.cat/GSOC16/virtio-vsock/netfilter
