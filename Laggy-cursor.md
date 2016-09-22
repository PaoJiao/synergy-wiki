###OS Info
Client: MacOS

### Problem
Using WIFI on MacOS client causes Synergy cursor laggy.

### Workaround
Run this command in the Terminal

`sudo ifconfig awdl0 down`

Then type `ifconfig` to double check the status under the awdl0 section, it should be `status: inactive`.
