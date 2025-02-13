******************************************
Setup Linux Toolchain from Scratch
******************************************

:link_to_translation:`zh_CN:[中文]`

The following instructions are alternative to downloading binary toolchain from Espressif website. To quickly setup the binary toolchain, instead of compiling it yourself, backup and proceed to section :doc:`linux-setup`.

.. note:: The reason you might need to build your own toolchain is to solve the Y2K38 problem (time_t expand to 64 bits instead of 32 bits).

Install Prerequisites
=====================

To compile with ESP-IDF you need to get the following packages:

- CentOS 7::

    sudo yum -y update && sudo yum install git wget ncurses-devel flex bison gperf python3 python3-pip cmake ninja-build ccache dfu-util libusbx

CentOS 7 is still supported but CentOS version 8 is recommended for a better user experience.

- Ubuntu and Debian::

    sudo apt-get install git wget libncurses-dev flex bison gperf python3 python3-pip python3-setuptools python3-serial python3-cryptography python3-future python3-pyparsing python3-pyelftools cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0

- Arch::

    sudo pacman -Sy --needed gcc git make ncurses flex bison gperf python-pyserial python-cryptography python-future python-pyparsing python-pyelftools cmake ninja ccache dfu-util libusb

.. note::
    CMake version 3.5 or newer is required for use with ESP-IDF. Older Linux distributions may require updating, enabling of a "backports" repository, or installing of a "cmake3" package rather than "cmake".

Compile the Toolchain from Source
=================================

- Install dependencies:

  - CentOS 7::

        sudo yum install gawk gperf grep gettext ncurses-devel python3 python3-devel automake bison flex texinfo help2man libtool make

  - Ubuntu pre-16.04::

        sudo apt-get install gawk gperf grep gettext libncurses-dev python python-dev automake bison flex texinfo help2man libtool make

  - Ubuntu 16.04 or newer::

        sudo apt-get install gawk gperf grep gettext python python-dev automake bison flex texinfo help2man libtool libtool-bin make

  - Debian 9::

        sudo apt-get install gawk gperf grep gettext libncurses-dev python python-dev automake bison flex texinfo help2man libtool libtool-bin make

  - Arch::

        sudo pacman -Sy --needed python-pip

Create the working directory and go into it::

  mkdir -p ~/esp
  cd ~/esp

Download ``crosstool-NG`` and build it:

.. include-build-file:: inc/scratch-build-code.inc

Build the toolchain::

    ./ct-ng {IDF_TARGET_TOOLCHAIN_PREFIX}
    ./ct-ng build
    chmod -R u+w builds/{IDF_TARGET_TOOLCHAIN_PREFIX}

Toolchain will be built in ``~/esp/crosstool-NG/builds/{IDF_TARGET_TOOLCHAIN_PREFIX}``.

Add Toolchain to PATH
=====================

The custom toolchain needs to be copied to a binary directory and added to the ``PATH``. Choose a directory, for example ``~/esp/{IDF_TARGET_TOOLCHAIN_PREFIX}/``, and copy the build output to this directory.

To use it, you will need to update your ``PATH`` environment variable in ``~/.profile`` file. To make ``{IDF_TARGET_TOOLCHAIN_PREFIX}`` available for all terminal sessions, add the following line to your ``~/.profile`` file::

    export PATH="$HOME/esp/{IDF_TARGET_TOOLCHAIN_PREFIX}/bin:$PATH"

.. note::

    If you have ``/bin/bash`` set as login shell, and both ``.bash_profile`` and ``.profile`` exist, then update ``.bash_profile`` instead. In CentOS, ``alias`` should set in ``.bashrc``.

Log off and log in back to make the ``.profile`` changes effective. Run the following command to verify if ``PATH`` is correctly set::

    printenv PATH

You are looking for similar result containing toolchain's path at the beginning of displayed string::

    $ printenv PATH
    /home/user-name/esp/{IDF_TARGET_TOOLCHAIN_PREFIX}/bin:/home/user-name/bin:/home/user-name/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

Instead of ``/home/user-name`` there should be a home path specific to your installation.

Next Steps
==========

To carry on with development environment setup, proceed to :ref:`get-started-get-esp-idf`.
