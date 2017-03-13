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
