German Server -> Windows 10 -> German keyboard layout
German Client -> Manjaro Linux -> German keyboard layout

If the client swaps z and y, and can't write german letters correctly (öäüß) type "setxkbmap de" in terminal.

Now it works correctly.

I could not get it to work in my i3 or KDE session with .xinitrc.
That's why I created a script like this, run at x login:

`cat setxkbmap.sh`

`#!/bin/bash`

`setxkbmap de`

`exit`