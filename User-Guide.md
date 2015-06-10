Intro
-----

Synergy lets you easily share your mouse and keyboard between multiple computers on your desk, and it’s Free and Open Source. Just move your mouse off the edge of one computer’s screen on to another. You can even share all of your clipboards. All you need is a network connection. Synergy is cross-platform (works on Windows, Mac OS X and Linux).

```
[[images/ManualIntro.jpg]] 
Image needs to be added
```

This user guide is useful for setting up Synergy. For answers to common problems, please see the [[Synergy FAQ]]. This user guide is written for Synergy version 1.4 – please upgrade if you are using an older version (like 1.3). To ﬁnd your version, click the "Help" menu, and select "About" or "About Synergy".

More information and download links can be found at [our website](http://synergy-project.org).

Install
-------

Once you have downloaded the latest version of Synergy from our downloads page, follow the instructions below.

## Windows
To install Synergy on Windows, follow these steps:
* Double click the downloaded installer.
* Click "I Agree" to agree to the license.
* You can choose an install location, but we recommend you use the default.
* Click "Install" to begin the installation.
* Once ﬁnished, click "Close" to ﬁnish the installation.

If this is your ﬁrst installation, the setup wizard should appear when you ﬁrst run Synergy. The wizard may also appear if we've changed something or added a new feature. Note that on Windows the wizard may have started behind another window. Once you have installed Synergy on all of your machines, skip to the "Conﬁgure" section of this guide.

## Mac OS X
To install Synergy on Mac OS X, follow these steps:
* Open the .dmg ﬁle you downloaded.
* Drag the Synergy app into the Applications shortcut in the same window.
* If prompted, click "Replace".
* Open your Applications folder
* Find the new Synergy app and double click.

If this is your ﬁrst installation, the setup wizard should appear when you ﬁrst run Synergy. The wizard may also appear if we've changed something or added a new feature. Once you have installed Synergy on all of your machines, skip to the "Conﬁgure" section of this guide.

## Linux
To install Synergy on Linux, follow these steps:
* Double click the .deb or .rpm ﬁle.
* Follow any on-screen install instructions for your distro.
* Once complete, you can ﬁnd the Synergy program in Accessories.

If this is your first installation, the setup wizard should appear when you ﬁrst run Synergy. The wizard may also appear if we've changed something or added a new feature. Once you have installed Synergy on all of your machines, skip to the "Conﬁgure" section of
this guide.

Configure
---------

## The Wizard
The wizard is designed to be easy enough to understand without reading a user guide.

The wizard will not fully conﬁgure Synergy. You can learn how to conﬁgure Synergy by reading the remainder of this section.

## Configure a Server

The server computer will share its keyboard and mouse with clients. It needs to know about all clients that will connect to it. To tell the server about these clients, click "Configure Server".

* To add a new client, drag a new screen onto the grid from the spare at the upper right.
* To remove a client, drag the screen to the trash can in the upper left.
* To move a client, drag the existing screen to another cell.
* If you drag a screen on top of another screen, the screens will switch places.
Note: This tool imposes a 5x3 limit of screens, though it is possible to have more screens
by editing the [[conﬁguration file|Text Config]] manually.

Once you have added a client to your grid, it will have the name "Unnamed". Double-click the icon to change its name to the name of your client.

You must use the client name that Synergy shows on the client. This can be found by looking at the main window of the Synergy application running on your client. The line labeled "Screen Name" under the Client Section is where you find the name that your computer calls itself.

Once you have entered the client name, click "OK" to return to the grid. Repeat this process for each client you wish to add. Once you have added all clients, click "OK" to return to the main window.

Now you need to click "Apply" to activate the conﬁguration. If the Apply button is disabled, click Start.

At this point you should note the "IP Addresses" line at the top of the main Synergy window. You will use one of the IP Addresses listed here on each Client computer to connect it to the Server.

_Note: A Synergy installation will have a default port number set for connecting (client) or listening (server). The server port on the Synergy server must not be blocked by a firewall (see the [[Troubleshooting]] page). Make sure the port the Synergy server is using allows incoming connections and that it is the same port number listed in the Synergy client (see [Configure a Client](#configure-a-client))_

## Configure a Client
Once you've conﬁgured your Server, you need to connect each Client to the server. Assuming you selected "Client" during the wizard, the Client option should be checked on the main window. In the "Server IP" field, enter the IP address of the server. This will be the one from the Server's Synergy screen, in the field named "IP Addresses".
Note: This field will also accept a hostname if you have DNS conﬁgured on your local network (most users do not).

Click Apply or Start.

_Note: The port number on the Synergy client must be set to the same number as the port number on the Synergy server. You may check this by going to Edit --> Settings (Win/Linux) or Synergy --> Preferences to display what port numbers each are set._

Startup on Boot
---------------

## Windows
This is done automatically during install. Synergy sets up a System Service which runs Synergy automatically. This allows Synergy to be used to login to Windows.

## Mac and Linux
There is currently no way to login to a Mac or Linux through Synergy. We have not found a good way to do this. The closest we can get is to set up Synergy as a login item, and then have your user auto-login to OS X or Linux.

Troubleshooting
---------------

### My Client and Server both state they are "connected" but my mouse does not leave the screen
Is your Scroll Lock key activated? This is useful for when playing video games or other full screen applications. The Scroll Lock key can be used to prevent the mouse switching input to another screen. Users that don't have a Scroll Lock key can re-assign the lock key by clicking "Conﬁgure Server" and selecting the "Hotkeys" tab.

### Why is the mouse over sensitive in games on the client?
To solve this you need to do two things.
First enable relative mouse movements on the server:
* Click "Conﬁgure Server",
* Click the "Advanced server settings" tab,
* Tick the "Use relative mouse moves" checkbox
* Click "OK"
If this doesn't work, you can try to lock the mouse to your client screen by pressing the Scroll Lock key. Press it again to unlock.

### Why do certain programs stop me from moving the mouse off screen?
This usually occurs in Vista and above, and is due to UAC in Windows. The solution to this is to run Synergy in "Elevated" mode (this can be selected at the bottom of the main Synergy window). You can also try launching Synergy as an administrator. Note that running Synergy in "Elevated" mode carries potential trade-offs with things like clipboard and file sharing functionality.

### How do I send developers the debug output?
Click the "Edit" menu and select "Settings". Then change the "Logging level" drop-down to "Debug" or higher. The debug output should appear in the "Log" area of the main window (if not, try restarting the program). You are able to copy text from this area by right clicking.

### There is some additional troubleshooting in the [[Synergy FAQ]].