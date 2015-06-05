# Building from Source Cent/OS 6.x
Tested on Cent/OS 6.6

## Pull from GitHub
`git clone git@github.com:synergy/synergy.git`


## Install prerequisites

Note: requires Qt4 and tools libraries

### As Root (su, sudo ...)

`yum install cmake make gcc-c++ libX11-devel libXext-devel libXi-devel libXtst-devel libXinerama-devel libcurl-devel qt-devel avahi-compat-libdns_sd-devel`

### As nonprivileged user

Set path for QT4

`PATH="$PATH:/usr/lib64/qt4/bin:/usr/lib/qt4/bin"`

### Configure script

Choose your build target 

`./hm.sh genlist`
`* 1: Unix Makefiles`
`* 2: Eclipse CTD4 - Unix Makefiles`

Configure (1 or 2 from list)

`./hm.sh setup-g 1`
`./hm.sh conf`
`./hm.sh build`



