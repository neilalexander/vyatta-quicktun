# QuickTun for Ubiquiti EdgeOS

### Introduction

This is a QuickTun distributable package for Ubiquiti EdgeOS, providing support for QuickTun TUN interfaces through the EdgeOS CLI. 

### Compatibility

|                       | Architecture | Compatible |                      Notes                     |
|-----------------------|:------------:|:----------:|:----------------------------------------------:|
|    EdgeRouter X (ERX) |    mipsel    |     Yes    | Builds with crossbuild-essential, see below    |

### Building for EdgeRouter X

On 64-bit Debian Jessie, start by installing the toolchain:
```
echo "deb http://emdebian.org/tools/debian/ jessie main" >> /etc/apt/sources.list

wget http://emdebian.org/tools/debian/emdebian-toolchain-archive.key
apt-key add emdebian-toolchain-archive.key

dpkg --add-architecture mipsel
apt-get update
apt-get install crossbuild-essential-mipsel
```
Compile the package then by cloning the repository and running 'make':
```
make
```
The package `vyatta-quicktun.deb` will be created in the parent directory. Copy it to the EdgeRouter and install it:
```
sudo dpkg -i vyatta-quicktun.deb
```

### Creating an interface

Create the interface by specifying protocol (either `raw`, `nacl0`, `nacltai` or `salty`), remote and local endpoints:
```
configure
set interfaces quicktun tun0 description "QuickTun Tunnel"
set interfaces quicktun tun0 protocol salty
set interfaces quicktun tun0 local address 1.1.1.1
set interfaces quicktun tun0 local port 1111
set interfaces quicktun tun0 remote address 2.2.2.2
set interfaces quicktun tun0 remote port 2222
commit
```
A keypair will automatically be generated if not specified:
```
configure
show interfaces quicktun tun0 private-key
show interfaces quicktun tun0 public-key
```

### Set tunnel interface addresses

Add IPv4 or IPv6 addresses to the virtual tunnel interface:
```
configure
set interfaces quicktun tun0 tunnel address 3.3.3.3/24
set interfaces quicktun tun0 tunnel address fd33:3333:3333:3333::3/64
commit
```

### Set firewall rules

Set any combination of firewall chains to be active on the virtual tunnel interface:
```
configure
set interfaces quicktun tun0 firewall in name CHAIN-IPv4-IN
set interfaces quicktun tun0 firewall local name CHAIN-IPv4-LOCAL
set interfaces quicktun tun0 firewall out name CHAIN-IPv4-OUT
set interfaces quicktun tun0 firewall in ipv6-name CHAIN-IPv6-IN
set interfaces quicktun tun0 firewall local ipv6-name CHAIN-IPv6-LOCAL
set interfaces quicktun tun0 firewall out ipv6-name CHAIN-IPv6-OUT
commit
```
