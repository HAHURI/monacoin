UNIXビルドノート
====================

UnixでMonacoin Coreをビルドする方法に関する注意事項。
（BSD固有の手順については、このディレクトリの `build-*bsd.md`を参照してください。）

ノート
---------------------
Monacoin Coreと依存関係を構成およびコンパイルするには、たとえば、依存関係のパスを指定する場合など、常に絶対パスを使用します。

```bash
	../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
```
ここで、BDB_PREFIXは絶対パスである必要があります-絶対パスの使用を保証する`$(pwd)`を使用して定義されます。

構築するには
---------------------

```bash
./autogen.sh
./configure
make
make install # optional
```

依存関係が満たされている場合、これによりmonacoin-qtもビルドされます。

依存関係
---------------------

これらの依存関係が必要です：

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 libssl      | Crypto           | Random Number Generation, Elliptic Curve Cryptography
 libboost    | Utility          | Library for threading, data structures, etc
 libevent    | Networking       | OS independent asynchronous networking

オプションの依存関係：

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 miniupnpc   | UPnP Support     | Firewall-jumping support
 libdb4.8    | Berkeley DB      | Wallet storage (only needed when wallet enabled)
 qt          | GUI              | GUI toolkit (only needed when GUI enabled)
 protobuf    | Payments in GUI  | Data interchange format used for payment protocol (only needed when GUI enabled)
 libqrencode | QR codes in GUI  | Optional for generating QR codes (only needed when GUI enabled)
 univalue    | Utility          | JSON parsing and encoding (bundled version will be used unless --with-system-univalue passed to configure)
 libzmq3     | ZMQ notification | Optional, allows generating ZMQ notifications (requires ZMQ version >= 4.x)

使用されるバージョンについては、[dependencies.md](dependencies.md)を参照してください

メモリ要件
--------------------

C ++コンパイラはメモリを大量に消費します。 Monacoin Coreのコンパイル時には、少なくとも1.5 GBのメモリを使用可能にすることをお勧めします。 より少ないシステムでは、追加のCXXFLAGSでメモリを節約するためにgccを調整できます。

```bash
./configure CXXFLAGS="--param ggc-min-expand=1 --param ggc-min-heapsize=32768"
```

## Linuxディストリビューション固有の手順

### Ubuntu & Debian

#### 依存関係のビルド手順

ビルド要件：

```bash
    sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev
```

ウォレットにはBerkeleyDBが必要です。


**Ubuntuのみ：** db4.8パッケージは[こちら](https://launchpad.net/~bitcoin/+archive/bitcoin)から入手できます。
次のコマンドを使用して、リポジトリを追加してインストールできます。

```bash
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev libdb4.8++-dev
```

UbuntuおよびDebianには独自のlibdb-devおよびlibdb ++-devパッケージがありますが、これらはBerkeleyDB 5.1以降をインストールします。これにより、BerkeleyDB 4.8に基づく分散実行可能ファイルとのバイナリウォレットの互換性が失われます。 ウォレットの互換性を気にしない場合は、configureに `--with-incompatible-bdb`を渡します。

ウォレットなしでモナコインコアを構築するには、「ウォレットモードを無効にする」セクションを参照してください。

オプション（--with-miniupnpcおよび--enable-upnp-defaultを参照）：

```bash
    sudo apt-get install libminiupnpc-dev
```

ZMQ依存関係（ZMQ API 4.xを提供）：

```bash
    sudo apt-get install libzmq3-dev
```

#### GUIの依存関係

monacoin-qtをビルドする場合は、Qt開発に必要なパッケージがインストールされていることを確認してください。 GUIを構築するにはQt 5が必要です。
GUIなしでビルドするには、 `--without-gui`を渡します。

Qt 5でビルドするには、次のものが必要です。

```bash
    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
```

libqrencode（オプション）は次のものでインストールできます。

```bash
    sudo apt-get install libqrencode-dev
```

これらがインストールされると、configureによって検出され、デフォルトでmonacoin-qt実行可能ファイルがビルドされます。


### Fedora

#### Dependency Build Instructions

Build requirements:

    sudo dnf install gcc-c++ libtool make autoconf automake openssl-devel libevent-devel boost-devel libdb4-devel libdb4-cxx-devel python3

Optional:

    sudo dnf install miniupnpc-devel

To build with Qt 5 you need the following:

    sudo dnf install qt5-qttools-devel qt5-qtbase-devel protobuf-devel

libqrencode (optional) can be installed with:

    sudo dnf install qrencode-devel

Notes
-----
The release is built with GCC and then "strip monacoind" to strip the debug
symbols, which reduces the executable size by about 90%.


miniupnpc
---------

[miniupnpc](http://miniupnp.free.fr/) may be used for UPnP port mapping.  It can be downloaded from [here](
http://miniupnp.tuxfamily.org/files/).  UPnP support is compiled in and
turned off by default.  See the configure options for upnp behavior desired:

	--without-miniupnpc      No UPnP support miniupnp not required
	--disable-upnp-default   (the default) UPnP support turned off by default at runtime
	--enable-upnp-default    UPnP support turned on by default at runtime


Berkeley DB
-----------
It is recommended to use Berkeley DB 4.8. If you have to build it yourself,
you can use [the installation script included in contrib/](/contrib/install_db4.sh)
like so

```shell
./contrib/install_db4.sh `pwd`
```

from the root of the repository.

**Note**: You only need Berkeley DB if the wallet is enabled (see the section *Disable-Wallet mode* below).

Boost
-----
If you need to build Boost yourself:

	sudo su
	./bootstrap.sh
	./bjam install


Security
--------
To help make your Monacoin Core installation more secure by making certain attacks impossible to
exploit even if a vulnerability is found, binaries are hardened by default.
This can be disabled with:

Hardening Flags:

	./configure --enable-hardening
	./configure --disable-hardening


Hardening enables the following features:

* Position Independent Executable
    Build position independent code to take advantage of Address Space Layout Randomization
    offered by some kernels. Attackers who can cause execution of code at an arbitrary memory
    location are thwarted if they don't know where anything useful is located.
    The stack and heap are randomly located by default, but this allows the code section to be
    randomly located as well.

    On an AMD64 processor where a library was not compiled with -fPIC, this will cause an error
    such as: "relocation R_X86_64_32 against `......' can not be used when making a shared object;"

    To test that you have built PIE executable, install scanelf, part of paxutils, and use:

    	scanelf -e ./monacoin

    The output should contain:

     TYPE
    ET_DYN

* Non-executable Stack
    If the stack is executable then trivial stack-based buffer overflow exploits are possible if
    vulnerable buffers are found. By default, Monacoin Core should be built with a non-executable stack,
    but if one of the libraries it uses asks for an executable stack or someone makes a mistake
    and uses a compiler extension which requires an executable stack, it will silently build an
    executable without the non-executable stack protection.

    To verify that the stack is non-executable after compiling use:
    `scanelf -e ./monacoin`

    The output should contain:
	STK/REL/PTL
	RW- R-- RW-

    The STK RW- means that the stack is readable and writeable but not executable.

Disable-wallet mode
--------------------
When the intention is to run only a P2P node without a wallet, Monacoin Core may be compiled in
disable-wallet mode with:

    ./configure --disable-wallet

In this case there is no dependency on Berkeley DB 4.8.

Mining is also possible in disable-wallet mode, but only using the `getblocktemplate` RPC
call not `getwork`.

Additional Configure Flags
--------------------------
A list of additional configure flags can be displayed with:

    ./configure --help


Setup and Build Example: Arch Linux
-----------------------------------
This example lists the steps necessary to setup and build a command line only, non-wallet distribution of the latest changes on Arch Linux:

    pacman -S git base-devel boost libevent python
    git clone https://github.com/monacoinproject/monacoin.git
    cd monacoin/
    ./autogen.sh
    ./configure --disable-wallet --without-gui --without-miniupnpc
    make check

Note:
Enabling wallet support requires either compiling against a Berkeley DB newer than 4.8 (package `db`) using `--with-incompatible-bdb`,
or building and depending on a local version of Berkeley DB 4.8. The readily available Arch Linux packages are currently built using
`--with-incompatible-bdb` according to the [PKGBUILD](https://projects.archlinux.org/svntogit/community.git/tree/bitcoin/trunk/PKGBUILD).
As mentioned above, when maintaining portability of the wallet between the standard Monacoin Core distributions and independently built
node software is desired, Berkeley DB 4.8 must be used.


ARM Cross-compilation
-------------------
These steps can be performed on, for example, an Ubuntu VM. The depends system
will also work on other Linux distributions, however the commands for
installing the toolchain will be different.

Make sure you install the build requirements mentioned above.
Then, install the toolchain and curl:

    sudo apt-get install g++-arm-linux-gnueabihf curl

To build executables for ARM:

    cd depends
    make HOST=arm-linux-gnueabihf NO_QT=1
    cd ..
    ./autogen.sh
    ./configure --prefix=$PWD/depends/arm-linux-gnueabihf --enable-glibc-back-compat --enable-reduce-exports LDFLAGS=-static-libstdc++
    make


For further documentation on the depends system see [README.md](../depends/README.md) in the depends directory.

