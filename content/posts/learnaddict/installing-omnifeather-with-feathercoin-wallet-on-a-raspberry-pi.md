---
title: "Installing OmniFeather with Feathercoin Wallet on a Raspberry Pi"
date: "2021-03-27"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/omnifeather/RPi4Agron.jpg"
  alt: "A picture of a Raspberry Pi and an Argon 4 case"
  relative: true
description: "Installing OmniFeather with Feathercoin Wallet on a Raspberry Pi"
---

[Feathercoin](https://feathercoin.com/) is one of the oldest cryptocurrencies and has a thriving community. It is actively being developed with new and modern features that allows it to compete with other coins like Bitcoin. Transactions are quickly confirmed, and it is safe and secure. If you are interested in helping the network through mining, Feathercoin is suited to hobbyist mining with reasonably fast graphics card.

The Feathercoin blockchain allows additional data to be stored within transactions, which can be used for things like token and contracts. The OmniFeather software client includes the ability to create tokens or Smart Properties, alongside a fully functioning Feathercoin wallet for sending and receiving coin transactions.

![Omnifeather Description](/images/learnaddict/omnifeather/OmniFeatherDescription.png)

{{< youtube xm3e-Zy2p2c >}}

In this tutorial, we are installing OmniFeather on a Raspberry Pi 4, but it may also work on a Raspberry Pi 3, as well as Ubuntu and Debian computers. The OmniFeather client requires to verify the ~4GB blockchain whilst it is being downloaded, using high processor and memory resources, which may be a problem depending on the specification of your hardware. Compiling the software from source code also requires a lot of resources, so something to keep an eye on.

The Raspberry Pi used for writing this tutorial is one of the best possible specifications currently available. This demonstrates that the Raspberry Pi is capable of running OmniFeather and lower specification Raspberry Pis may also provide similar results.

The Raspberry Pi specification is as follows:

- [Raspberry Pi 4 Model B with 8GB RAM](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) (overclocked to 2.0GHz)
- [Argon ONE M.2 Raspberry Pi 4 Case](https://www.argon40.com/argon-one-m-2-case-for-raspberry-pi-4.html)
- [Official Raspberry Pi 4 Power Supply (5.1V 3A)](https://www.raspberrypi.org/products/type-c-power-supply/)
- [WD Green SATA SSD M.2 2280 240GB](https://shop.westerndigital.com/en-gb/products/internal-drives/wd-green-sata-m-2-ssd#WDS240G2G0B)

I am using the [Raspberry Pi OS with desktop](https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit) image which was written to the M.2 SSD. It boots over USB3, which required the [boot order changing](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/msd.md). The commands below are typed into into the Linux command line console, which can be found on the Raspberry Pi OS menu under Accessories -> Terminal.

It’s good practice to check for updates and apply any upgrades before installing new packages. The following two commands will ensure the information about available packages is up to date and then apply any upgrades that may be available. You may be prompted whether you would like to continue whilst upgrading and installing new packages.

```bash
sudo apt-get update
sudo apt-get full-upgrade
```

We can now start installing some supporting packages. The first set are essential for generally building software from source code and the second set are specific dependencies for OmniFeather.

```bash
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3 git
sudo apt-get install libssl-dev libevent-dev libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev
```

The graphical application (GUI) also requires some additional packages to be installed. The following will install the QT Framework and some other dependencies.

```bash
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
```

There’s a couple more optional packages, which I installed as well.

```bash
sudo apt-get install libqrencode-dev libzmq3-dev libminiupnpc-dev
```

The OmniFeather source code is available on GitHub and we will use the following command to download the OmniFeather repository from GitHub to your Raspberry Pi.

```bash
git clone https://github.com/OmniLayer/omnifeather.git
```

Next, we need to install the Berkeley DB software libraries. The packaged version available for the Raspberry Pi is newer than the supported version, so we will need to use the following install script to download and install the libraries separately. The install script is available in the OmniFeather repository that we just downloaded.

```bash
cd omnifeather
./contrib/install_db4.sh `pwd`
```

The output from the db4 build script provides a hint for running the configure file later on, once we have generated the file. Unfortunately, this is not compatible on this operating system, so alternative lines will be provided below.

![Output from the db4 build process.](/images/learnaddict/omnifeather/DB_Complete.png)

We next need to run the autogen.sh script in order to generate a configure file for OmniFeather. This is standard procedure for installing C++ programs and is to prepare for compiling the software.

```bash
./autogen.sh
```

We can now run the generated configure file with some additional parameters. Note: This is different to what the screenshot shows above.

```bash
export BDB_PREFIX=`pwd`/db4
BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include" ./configure
```

![A completed autogen.sh output.](/images/learnaddict/omnifeather/autogen_Complete.png)

Let’s now compile the source code into programs you can actually run. This may take several hours so please be patient!

```bash
make
```

![The last output from the make command when successful.](/images/learnaddict/omnifeather/make_Complete.png)

Once the make command has finished compiling, check for any error messages shown in your console window. Any errors would be in the last few lines before it finished. Occasionally, you may see some warnings go past, but these can be ignored.

If you have been successful in compiling the software, you can now install the software using the make install command. This should take no more than a few minutes to run.

```bash
sudo make install
```

If everything has been successful so far, you should be able to run the OmniFeather application now. It can be run from the console using the following command.

```bash
omnifeather-qt
```

Note: Downloading the blockchain requires a lot of calculations to be carried out whilst verifying the headers and blocks. This will warm up your Raspberry Pi, so make sure you have some sort of cooling and keep an eye on it. OmniFeather can be closed at any time and it will continue to download the next time you launch it.

![Selecting the data directory](/images/learnaddict/omnifeather/onmifeather_Welcome.png)

The default data directory is usually a good choice for most people. There needs to be around 4GB of free space available. It is possible to change this location if you have some fast external storage, for example.

OmniFeather requires to download a copy of the Feathercoin blockchain. This starts with syncing the headers and then continues to download the blocks that make up the blockchain. Each block contains all the transactions made a within a certain one minute duration, and we need to download all the blocks since 2013.

This can take a while, depending on your Raspberry Pi configuration, speed of storage, and Internet bandwidth. There is a Hide button available, but you will need to download the entire blockchain before you can start making any transactions. The status will still show in the bottom left of the application, if you do hide the status screen.

![Syncing the headers.](/images/learnaddict/omnifeather/syncing_Headers.png)

Once OmniFeather has completed the download, it will show the full application. You are now able to send and receive Feathercoin transactions.

![Omnifeather Overview screen](/images/learnaddict/omnifeather/omnifeather_Ready.png)

When you close the OmniFeather application, it will display a small window saying it is shutting down. Please wait until this disappears before powering off your Raspberry Pi to avoid problems with corruption, etc.

![Shutting down…](/images/learnaddict/omnifeather/omnifeather_ShuttingDown.png)

The next time you open the OmniFeather application, it may take a moment to load the block index. This shouldn’t take too long to complete.

![Loading block index…](/images/learnaddict/omnifeather/omnifeather_LoadingScreen.png)

OmniFeather creates you a new Wallet containing information that secures your transactions on the blockchain. If you lose this file, you won’t be able to recover any tokens or coins you own, so please do create a backup copy and keep it safe.

The location of wallet.dat is shown below. You can encrypt your Wallet within the OmniFeather application to keep it safe, just don’t forget your password. Any backup copies of wallet.dat taken prior to encryption will still be valid and may be used to access your Wallet without a password, so keep all backups in a safe place.

![wallet.dat file location](/images/learnaddict/omnifeather/omnifeather_Wallet.png)

So, what’s next? Feathercoin is traded on [Bittrex](https://international.bittrex.com/Market/Index?MarketName=BTC-FTC), where other cryptocurrencies and fiat currencies, like US Dollars and Euros, can be used to buy Feathercoins. More information is available on the [Feathercoin website](https://feathercoin.com/). Why not check in with the community and say hello! [Forum](https://forum.feathercoin.com/), [Discord](https://t.me/FeathercoinOfficial), [Telegram](https://t.me/FeathercoinOfficial), and [Reddit](https://www.reddit.com/r/FeatherCOin/).
