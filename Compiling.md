Introduction
------------

This guide is for Synergy version 1.3.5 and above. First you need to download the [Source-Code], then install the [dependencies](#dependencies), and finally follow the [compile instructions](#compile).

Dependencies
------------

### Windows

#### Windows XP (and above)

-   [Visual C++ Express 2008] (or VC++ 6.0 and above)
-   [CMake]
-   [Python]
-   [Qt SDK 2010.02] (provides qmake and MinGW used for building
    the GUI. Note: use the MinGW version provided with Qt as a mismatch
    can cause random crashes)
-   [Wix] v3.8 (for building the windows installer, since Synergy
    version 1.4.17)

You may need to add your Python install directory to the end of your
PATH environment variable (System &gt; Advanced System Settings &gt;
Environment Variables) so that `hm` can be called from the command
prompt.

Using Python 3.x will result in syntax errors as print became a function
rather than a statement: [What's new in Python 3.0]

##### Windows x64

-   All dependencies required for Windows x86
-   Visual Studio 2008 (x64) \*SP1\* or Visual Studio 2005 (x64) \*SP1\*
-   64-bit compiler extensions installed (you may need to re-run Visual
    Studio setup)

  [Visual C++ Express 2008]: http://www.microsoft.com/express/vc/
  [CMake]: http://www.cmake.org/cmake/resources/software.html
  [Python]: http://www.python.org/download/
  [Qt SDK 2010.02]: http://synergy-foss.org/mirror/qt-sdk-win-opensource-2010.02.exe
  [Wix]: http://wixtoolset.org/
  [What's new in Python 3.0]: http://docs.python.org/3.0/whatsnew/3.0.html#print-is-a-function

### Mac OS X

#### Mac OS X 10.10 Yosemite

-   Install cmake and Qt as usual (e.g. using brew):

<!-- -->

    brew install cmake
    brew install qt

-   Fix `./ext/toolchain/commands1.py`:

In the function configure\_core(), at approx line 433, change

`elif sys.platform == "darwin": ` to read `if sys.platform == "darwin":`

In the function macPostGuiMake(), at approx line 770 and/or 773 change
the path to be what your Qt library path is, probably,

`frameworkRootDir = "/usr/local/Cellar/qt/4.8.7/Frameworks"`

See [this github issue] for more information.

-   Now configure and build:

<!-- -->

    ./hm.sh conf -g1 --mac-sdk 10.10 --mac-identity test
    ./hm.sh build

Credits to [this blog entry]

#### Mac OS X 10.9 Mavericks

[Install Command Line Tools In OSX 10.9 Mavericks]

#### Mac OS X 10.4 (and above)

-   [XCode] (\*with command line tools)
-   [Qt SDK 2010.03] (provides qmake)

To install the command line tools, Xcode &gt; Preferences &gt; Downloads
and install component named “Command Line Tools”.

If you are missing the `/Developer` directory, you may need to run
(assuming the path is correct):

    mkdir /Developer
    cd /Developer
    ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Library
    ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/SDKs

**Note:** For Mac OS X 10.5 and above, Python should already be
installed. Otherwise, visit the [CMake] and [Python] download pages.

**Note:** Upgrading Mac OS X may cause the command line tools and Qt
libraries to be removed (even on minor upgrades), so you may have to
reinstall both each time after doing this.

  [this github issue]: https://github.com/synergy/synergy/issues/4572
  [this blog entry]: https://wordpress.update.sh/archives/410
  [Install Command Line Tools In OSX 10.9 Mavericks]: http://www.computersnyou.com/2025/2013/06/install-command-line-tools-in-osx-10-9-mavericks-how-to/
  [XCode]: http://developer.apple.com/technology/xcode.html
  [Qt SDK 2010.03]: http://synergy-foss.org/mirror/qt-sdk-mac-opensource-2010.03.dmg
  [CMake]: http://www.cmake.org/cmake/resources/software.html
  [Python]: http://www.python.org/download/

#### Using “brew” method

[Brew] for Mac OS X is an alternative to MacPorts and Fink.

    brew install cmake

#### Using “macports” method

If you already have [MacPorts], run:

    sudo port install cmake

### Ubuntu 10.04 LTS

    sudo apt-get install cmake make g++ xorg-dev libqt4-dev libcurl4-openssl-dev libavahi-compat-libdnssd-dev libssl-dev

### Ubuntu 15.04

    sudo apt-get install cmake make g++ xorg-dev libqt4-dev libcurl4-openssl-dev libavahi-compat-libdnssd-dev libssl-dev

### CentOS 6.5

    su
    yum install cmake make gcc-c++ libX11-devel libXext-devel libXi-devel libXtst-devel libXinerama-devel libcurl-devel qt-devel avahi-compat-libdns_sd-devel openssl-devel
    PATH="$PATH:/usr/lib64/qt4/bin:/usr/lib/qt4/bin"

### Fedora 20

    sudo yum install cmake make gcc-c++ libX11-devel libXtst-devel libXext-devel libXinerama-devel libcurl-devel qt-devel avahi-compat-libdns_sd-devel openssl-devel
    PATH="$PATH:/usr/lib64/qt4/bin:/usr/lib/qt4/bin"

### OpenSUSE 11.1

    su
    yast2 -i cmake python gcc-c++ xorg-x11-devel libcurl-devel (SSL dev package, not sure what it's called on SuSE)

### Mandriva One 2009

    su
    urpmi cmake python gcc-c++ make xorg-x11-dev libcurl-dev (SSL dev package, not sure what it's called on Mandriva)

### OpenSolaris 2009.06

    su
    pkg install SUNWPython SUNWcmake SUNWgcc SUNWxorg-headers SUNWxwinc libcurl-devel (SSL dev package, not sure what it's called on Solaris)

### Unix or other Linux

-   CMake
-   Python
-   GNU Make
-   GCC (2.95 and up)
-   X11R4 and up (headers and libraries)
-   Xtst (e.g. libXtst-devel)
-   Qt 2010.03
-   libCURL
-   SSL dev package

  [Brew]: http://mxcl.github.com/homebrew/
  [MacPorts]: http://www.macports.org/install.php

Compile
-------

Use the `hm` (or `./hm.sh`) script located the root of our project
directory. This is just a wrapper for `cmake`, so you don't need to use
it, however we recommend you do. The `conf` command will generate the
projects in the `build` directory (which can be opened manually and used
for compiling).

Use the `hm genlist` command to see other generators. The `-g1` argument (shown below) will cause the 1st CMake generator to be used. You may want to use other generators if you are building with Eclipse, for example.

With no extra arguments, release build will be compiled. If you need the
debug release, use the `-d` argument.

  [dependencies]: #Dependencies "wikilink"
  [Source Code]: Source_Code "wikilink"
  [legacy compile guide]: http://synergy2.sourceforge.net/compiling.html

### Windows

    hm conf -g1
    hm build

### Unix, Linux, Mac OS X

    ./hm.sh conf -g1 [-d] # Use -d to build a debug version.
    ./hm.sh build [-d]

After compiling the binaries can be found in the `bin` directory in
the root of the source tree (not in the build directory).

Building installers
-------------------

Use the `hm package` command to build installers. Of course you can only
build those that are applicable for your platform.

  [Visual C++ Express 2008]: http://www.microsoft.com/express/vc/
  [Code::Blocks]: http://www.codeblocks.org/
  [Eclipse CDT]: http://www.eclipse.org/cdt/