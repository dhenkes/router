# Router

This repository contains the config files to build a simple Debian based router
/ firewall.

It is recommended to use Debian 10 or higher (due to nftables) and a machine
with two or more ethernet ports.

## Network

The configs are specific to my network so some things need to be updated or
changed according to your setup.

### Ports

I use a PC Engines APU2D4 as my router. It has the following ethernet ports:

- `enp1s0`: WAN
- `enp2s0`: LAN (Connected to a switch)
- `enp3s0`: LAN (Connected to an AP)

### VLANs

This VLAN configuration served me well for some time now but new VLANs can be
added or not needed ones removed.

- `VLAN10`: Management
- `VLAN11`: Servers
- `VLAN20`: General
- `VLAN30`: Guest
- `VLAN40`: Camera
- `VLAN50`: IoT - Hue

### IPv6

By following this setup you will add IPv6 support to VLAN10. If this is not
wanted just do not install the `wide-dhcpv6-client` package and change the
other configs accordingly.

## Setup

We should make sure that the machine is up-to-date and contains at least an
editor for us to work with. `vim` can be substituted by your editor of choice.

```
apt update
apt upgrade
apt install vim
```

### Router specific packages

After everything is updated and installed, we can go ahead and add the router
specific packages.

```
apt install vlan bridge-utils dnsmasq nftables wide-dhcpv6-client
```

These packages are required for the router to do the basic work.

- `vlan`: ifupdown integration for vlan configuration
- `bridge-utils`: Linux ethernet bridge configuration utilities
- `dnsmasq`: DNS proxy and DHCP server
- `nftables`: Packet filtering / firewall
- `wide-dhcpv6-client`: DHCPv6 client for automatic IPv6 host configuration

## Enable 8021q support

The last thing we have to do is enable 8021q support. It is the networking
standard that supports virtual LANs on a network.

```
/usr/sbin/modprobe 8021q
echo '8021q' >> /etc/modules
```

## Configs

The configs should be self-explanatory, if there are any questions just create
an issue and I will try to answer / add the explanation to this README.
