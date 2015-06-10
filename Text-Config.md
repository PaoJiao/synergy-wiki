About textual configuration files
---------------------------------

Most of the time, needing to deal with text-based configuration files directly can be avoided by using the GUI, however there are times when a text config is necessary.

When a text-based configuration file is needed you can specify it on the command-line using the `--config` option. Example: `synergys --config /path/to/synergy.conf`   Synergy also checks certain pathnames to load the configuration file if you don't specify a path using the `--config`. Running `synergys --help` reports those pathnames.

If you need to access a feature that is not available in the GUI, but otherwise have it working, you can get a good starting point by exporting your config from the GUI by using the File -> Save Configuration As... option.

Why would I want to use a text-based config?
--------------------------------------------

The number one reason is that you don't really have a choice. If you aren't using a gui than you have to use a text-based config file.

The second reason is that it gives you more control. The GUI cannot create advanced configuration such as non-reciprocal connections. An example of a non-reciprocal connection would be that if you go right from screen A you get to screen B, but if you then go left from screen B you get to screen C rather than back to screen A as you would in a reciprocal connection.

Other considerations:
* You can use a version control system
* It's easier to share configs with people
* You can have multiple config files (like if you use a laptop in multiple places)

File Format
-----------

The configuration file is a plain text file. Use any text editor to create the configuration file. The file is broken into sections and each section has the form:

```
 section: _name_
 	_args_
 end
```

Comments are introduced by _`#`_ and continue to the end of the line. _`name`_ must be one of the following:

* _`screens`_
* _`aliases`_
* _`links`_
* _`options`_

See below for further explanation of each section type. The configuration file is case-sensitive so `Section`, `SECTION`, and `section` are all different and only the last is valid. Screen names are the exception; screen names are case-insensitive.

The file is parsed top to bottom and names cannot be used before they've been defined in the `screens` or `aliases` sections. So the `links` and `aliases` must appear after the `screens` and `links` cannot refer to aliases unless the `aliases` appear before the `links`.

### screens

_`args`_ is a list of screen names, one name per line, each followed by a colon. Names are arbitrary strings but they must be unique. The hostname of each computer is recommended. (This is the computer's network name on win32 and the name reported by the program hostname on Unix and OS X. Note that OS X may append .local to the name you gave your computer; e.g. somehost.local.) There must be a screen name for the server and each client. Each screen can specify a number of options. Options have the form name = value and are listed one per line after the screen name.

 section: screens
     moe:
     larry:
         halfDuplexCapsLock = true
         halfDuplexNumLock = true
     curly:
         meta = alt
 end

This declares three screens named _`moe`_, _`larry`_, and _`curly`_. Screen _`larry`_ has half-duplex Caps Lock and Num Lock keys (see below) and screen _`curly`_ converts the meta modifier key to the alt modifier key.

#### screen options

A screen can have the following options:

* `halfDuplexCapsLock = {true|false}`
: This computer has a Caps Lock key that doesn't report a press and a release event when the user presses it but instead reports a press event when it's turned on and a release event when it's turned off. If Caps Lock acts strangely on all screens then you may need to set this option to true on the server screen. If it acts strangely on one screen then that screen may need the option set to true.

* `halfDuplexNumLock = {true|false}`  
   This is identical to `halfDuplexCapsLock` except it applies to the Num Lock key.

* `halfDuplexScrollLock = {true|false}`  
   This is identical to `halfDuplexCapsLock` except it applies to the Scroll Lock key. Note that, by default, synergy uses Scroll Lock to keep the cursor on the current screen. That is, when Scroll Lock is toggled on, the cursor is locked to the screen that it's currently on. You can use that to prevent accidental switching. You can also configure other hot keys to do that; see lockCursorToScreen.

* `switchCorners = &lt;corners&gt;`  
   See switchCorners below.

* `switchCornerSize = N`  
   See switchCornerSize below.

* `xtestIsXineramaUnaware = {true|false}`  
   This option works around a bug in the XTest extension when used in combination with Xinerama. It affects X11 clients only. Not all versions of the XTest extension are aware of the Xinerama extension. As a result, they do not move the mouse correctly when using multiple Xinerama screens. This option is currently _`true`_ by default. If you know your XTest extension is Xinerama aware then set this option to _`false`_.

* `shift = {shift|ctrl|alt|meta|super|none}`  
  `ctrl = {shift|ctrl|alt|meta|super|none}`  
  `alt = {shift|ctrl|alt|meta|super|none}`  
  `meta = {shift|ctrl|alt|meta|super|none}`  
  `super = {shift|ctrl|alt|meta|super|none}`  
   Map a modifier key pressed on the server's keyboard to a different modifier on this client. This option only has an effect on a client screen; it's accepted and ignored on the server screen.
:You can map, say, the shift key to shift (the default), ctrl, alt, meta, super or nothing. Normally, you wouldn't remap shift or ctrl. You might, however, have an X11 server with meta bound to the Alt keys. To use this server effectively with a windows client, which doesn't use meta but uses alt extensively, you'll want the windows client to map meta to alt (using `meta = alt`).

### aliases

_`args`_ is a list of screen names just like in the _`screens`_ section except each screen is followed by a list of aliases, one per line, not followed by a colon. An alias is a screen name and must be unique. During screen name lookup each alias is equivalent to the screen name it aliases. So a client can connect using its canonical screen name or any of its aliases.

```
 section: aliases
     larry:
         larry.stooges.com
     curly:
         shemp
 end
```

Screen _`larry`_ is also known as _`larry.stooges.com`_ and can connect as either name. Screen _`curly`_ is also known as _`shemp`_ (hey, it's just an example).

### links

_`args`_ is a list of screen names just like in the _`screens`_ section except each screen is followed by a list of links, one per line. Each link has the form `{left|right|up|down}[<range>] = name[<range>]`. A link indicates which screen is adjacent in the given direction.

Each side of a link can specify a range which defines a portion of an edge. A range on the direction is the portion of edge you can leave from while a range on the screen is the portion of edge you'll enter into. Ranges are optional and default to the entire edge. All ranges on a particular direction of a particular screen must not overlap.

A <range> is written as `(<start>,<end>)`. Both ``start`_ and _`end`_ are percentages in the range 0 to 100, inclusive. The start must be less than the end. 0 is the left or top of an edge and 100 is the right or bottom.

```
 section: links
     moe:
         right        = larry
         up(50,100)   = curly(0,50)
     larry:
         left         = moe
         up(0,50)     = curly(50,100)
     curly:
         down(0,50)   = moe
         down(50,100) = larry(0,50)
 end
```

This indicates that screen _`larry`_ is to the right of screen _`moe`_ (so moving the cursor off the right edge of _`moe`_ would make it appear at the left edge of _`larry`_), the left half of curly is above the right half of _`moe`_, _`moe`_ is to the left of _`larry`_ (edges are not necessarily symmetric so you have to provide both directions), the right half of curly is above the left half of _`larry`_, all of _`moe`_ is below the left half of _`curly`_, and the left half of _`larry`_ is below the right half of _`curly`_.

Note that links do not have to be symmetrical; for instance, here the edge between _`moe`_ and _`curly`_ maps to different ranges depending on if you're going up or down. In fact links don't have to be bidirectional. You can configure the right of _`moe`_ to go to _`larry`_ without a link from the left of _`larry`_ to _`moe`_. It's possible to configure a screen with no outgoing links; the cursor will get stuck on that screen unless you have a hot key configured to switch off of that screen.

### options

_`args`_ is a list of lines of the form `name = value`. These set the global options.

```
 section: options
     heartbeat = 5000
     switchDelay = 500
 end
```

#### List of Options

* `heartbeat = N`  
   The server will expect each client to send a message no less than every `N` milliseconds. If no message arrives from a client within `3N` seconds the server forces that client to disconnect.  
  If synergy fails to detect clients disconnecting while the server is sleeping or vice versa, try using this option.

* `switchCorners = <corners>`  
   Synergy won't switch screens when the mouse reaches the edge of the screen if it's in a listed corner. The size of all corners is given by the `switchCornerSize` option.  
   Corners are specified by a list using the following names:
  * `none` -- no corners
  * `top-left` -- the top left corner
  * `top-right` -- the top right corner
  * `bottom-left` -- the bottom left corner
  * `bottom-right` -- the bottom right corner
  * `left` -- top and bottom left corners
  * `right` -- top and bottom right corners
  * `top` -- left and right top corners
  * `bottom` -- left and right bottom corners
  * `all` -- all corners
  
   The first name in the list is one of the above names and defines the initial set of corners. Subsequent names are prefixed with + or - to add the corner to or remove the corner from the set, respectively. For example: `all -left +top-left`  starts will all corners, removes the left corners (top and bottom) then adds the top-left back in, resulting in the top-left, bottom-left and bottom-right corners.

* `switchCornerSize = N`  
   Sets the size of all corners in pixels. The cursor must be within `N` pixels of the corner to be considered to be in the corner.

* `switchDelay = N`  
   Synergy won't switch screens when the mouse reaches the edge of a screen unless it stays on the edge for `N` milliseconds. This helps prevent unintentional switching when working near the edge of a screen.

* `switchDoubleTap = N`  
   Synergy won't switch screens when the mouse reaches the edge of a screen unless it's moved away from the edge and then back to the edge within `N` milliseconds. With the option you have to quickly tap the edge twice to switch. This helps prevent unintentional switching when working near the edge of a screen.

* `screenSaverSync = {true|false}`  
   If set to _`false`_ then synergy won't synchronize screen savers. Client screen savers will start according to their individual configurations. The server screen saver won't start if there is input, even if that input is directed toward a client screen.

* `relativeMouseMoves = {true|false}`  
   If set to _`true`_ then secondary screens move the mouse using relative rather than absolute mouse moves when and only when the cursor is locked to the screen (by Scroll Lock or a configured hot key). This is intended to make synergy work better with certain games. If set to _`false`_ or not set then all mouse moves are absolute.

* `keystroke(key) = actions`  
   Binds the _`key`_ combination key to the given _`actions`_. _`key`_ is an optional list of modifiers (_`shift`_, _`control`_, _`alt`_, _`meta`_ or _`super`_) optionally followed by a character or a key name, all separated by + (plus signs). You must have either modifiers or a character/key name or both. See below for [valid key names](#keyname) and [ actions](#Actions).
: Keyboard hot keys are handled while the cursor is on the primary screen and secondary screens. Separate actions can be assigned to press and release.

* `mousebutton(button) = actions`  
   Binds the modifier and mouse button combination _`button`_ to the given _`actions`_. _`button`_ is an optional list of modifiers (_`shift`_, _`control`_, _`alt`_, _`meta`_ or _`super`_) followed by a button number. The primary button (the left button for right handed users) is button 1, the middle button is 2, etc. Actions can be [found below](#Actions)  
   Mouse button actions are not handled while the cursor is on the primary screen. You cannot use these to perform an action while on the primary screen. Separate actions can be assigned to press and release.

You can use both the `switchDelay` and `switchDoubleTap` options at the same time. Synergy will switch when either requirement is satisfied.

#### Actions

Actions are two lists of individual actions separated by commas. The two lists are separated by a semicolon. Either list can be empty and if the second list is empty then the semicolon is optional. The first list lists actions to take when the condition becomes true (e.g. the hot key or mouse button is pressed) and the second lists actions to take when the condition becomes false (e.g. the hot key or button is released). The condition becoming true is called activation and becoming false is called deactivation. Allowed individual actions are:

* `keystroke(key[,screens])`

* `keyDown(key[,screens])`

* `keyUp(key[,screens])`  
   Synthesizes the modifiers and key given in _`key`_ which has the same form as described in the _`keystroke`_ option. If given, _`screens`_ lists the screen or screens to direct the event to, regardless of the active screen. If not given then the event is directed to the active screen only.  
   _`keyDown`_ synthesizes a key press and _`keyUp`_ synthesizes a key release. _`keystroke`_ synthesizes a key press on activation and a release on deactivation and is equivalent to a _`keyDown`_ on activation and _`keyUp`_ on deactivation.  
   _`screens`_ is either * to indicate all screens or a colon (:) separated list of screen names. (Note that the screen name must have already been encountered in the configuration file so you'll probably want to put actions at the bottom of the file.)

* `mousebutton(button)`
* `mouseDown(button)`
* `mouseUp(button)`  
   Synthesizes the modifiers and mouse button given in _`button`_ which has the same form as described in the _`mousebutton`_ option.  
   _`mouseDown`_ synthesizes a mouse press and _`mouseUp`_ synthesizes a mouse release. _`mousebutton`_ synthesizes a mouse press on activation and a release on deactivation and is equivalent to a _`mouseDown`_ on activation and _`mouseUp`_ on deactivation.

* `lockCursorToScreen(mode)`  
   Locks the cursor to or unlocks the cursor from the active screen. _`mode`_ can be _`off`_ to unlock the cursor, _`on`_ to lock the cursor, or _`toggle`_ to toggle the current state. The default is _`toggle`_. If the configuration has no `lockCursorToScreen` action and Scroll Lock is not used as a hot key then Scroll Lock toggles cursor locking.

* `switchToScreen(screen)`  
   Jump to screen with name or alias _`screen`_.

* `switchInDirection(dir)`  
   Switch to the screen in the direction _`dir`_, which may be one of _`left`_, _`right`_, _`up`_ or _`down`_.

#### Keynames 
Valid key names are:
* AppMail
* AppMedia
* AppUser1
* AppUser2
* AudioDown
* AudioMute
* AudioNext
* AudioPlay
* AudioPrev
* AudioStop
* AudioUp
* BackSpace
* Begin
* Break
* Cancel
* CapsLock
* Clear
* Delete
* Down
* Eject
* End
* Escape
* Execute
* F1
* F2
* F3
* F4
* F5
* F6
* F7
* F8
* F9
* F10
* F11
* F12
* F13
* F14
* F15
* F16
* F17
* F18
* F19
* F20
* F21
* F22
* F23
* F24
* F25
* F26
* F27
* F28
* F29
* F30
* F31
* F32
* F33
* F34
* F35
* Find
* Help
* Home
* Insert
* KP_0
* KP_1
* KP_2
* KP_3
* KP_4
* KP_5
* KP_6
* KP_7
* KP_8
* KP_9
* KP_Add
* KP_Begin
* KP_Decimal
* KP_Delete
* KP_Divide
* KP_Down
* KP_End
* KP_Enter
* KP_Equal
* KP_F1
* KP_F2
* KP_F3
* KP_F4
* KP_Home
* KP_Insert
* KP_Left
* KP_Multiply
* KP_PageDown
* KP_PageUp
* KP_Right
* KP_Separator
* KP_Space
* KP_Subtract
* KP_Tab
* KP_Up
* Left
* LeftTab
* Linefeed
* Menu
* NumLock
* PageDown
* PageUp
* Pause
* Print
* Redo
* Return
* Right
* ScrollLock
* Select
* Sleep
* Space
* SysReq
* Tab
* Undo
* Up
* WWWBack
* WWWFavorites
* WWWForward
* WWWHome
* WWWRefresh
* WWWSearch
* WWWStop
* Space
* Exclaim
* DoubleQuote
* Number
* Dollar
* Percent
* Ampersand
* Apostrophe
* ParenthesisL
* ParenthesisR
* Asterisk
* Plus
* Comma
* Minus
* Period
* Slash
* Colon
* Semicolon
* Less
* Equal
* Greater
* Question
* At
* BracketL
* Backslash
* BracketR
* Circumflex
* Underscore
* Grave
* BraceL
* Bar
* BraceR
* Tilde


Additionally, a name of the form `\uXXXX` where `XXXX` is a hexadecimal number is interpreted as a unicode character code. Key and modifier names are case-insensitive. Keys that don't exist on the keyboard or in the default keyboard layout will not work.

Example textual configuration file
----------------------------------

This example comes from doc/synergy-basic.conf

```
# sample synergy configuration file
#
# comments begin with the # character and continue to the end of
# line.  comments may appear anywhere the syntax permits.
# +-------+  +--------+ +---------+
# |Laptop |  |Desktop1| |iMac     |
# |       |  |        | |         |
# +-------+  +--------+ +---------+

section: screens
	# three hosts named:  Laptop, Desktop1, and iMac
	# These are the nice names of the hosts to make it easy to write the config file
	# The aliases section below contain the "actual" names of the hosts (their hostnames)
	Laptop:
	Desktop1:
	iMac:
end

section: links
	# iMac is to the right of Desktop1
	# Laptop is to the left of Desktop1
	Desktop1:
		right(0,100) = iMac # the numbers in parentheses indicate the percentage of the screen's edge to be considered active for switching)
		left  = Laptop
		shift = shift (shift, alt, super, meta can be mapped to any of the others)

	# Desktop1 is to the right of Laptop
	Laptop:
		right = Desktop1

	# Desktop1 is to the left of iMac
	iMac:
		left  = Desktop1
end

section: aliases
	# The "real" name of iMac is John-Smiths-iMac-3.local. 
	# If we wanted we could remove this alias and instead use John-Smiths-iMac-3.local everywhere iMac is above. 
	# Hopefully it should be easy to see why using an alias is nicer
	iMac:
		John-Smiths-iMac-3.local
end
```

Available options
-----------------

### Focus Preservation

The following section can be added to the screens section of the config (as shown) to let windows keep focus when you switch screens. This overrides the default option of making windows lose focus when you switch to a different machine. 
```
section: screens
	client1:
		preserveFocus = true # Don't drop focus when switching screens
		
end
```

### Cursor Wrapping

The text config allows screens to be wrapped around. For example, with two machines (a server and a client), the mouse can go off the right of the server onto the left side of the client, then off the right side of the client back onto the left side of server. This config also uses control+super+(left/right) to switch between machines on keypress.

```
# Physical monitor arrangement, with machine names as used by Synergy.
#  +----------+----------+
#  | syn-serv | syn-cli  |
#  |          |          |
#  +----------+----------+

section: screens
    syn-serv:
    syn-cli:
end
section: links
    syn-serv:
        left = syn-cli     # "wrapping" arrangement
        right = syn-cli    # "normal" arrangement
    syn-cli:
        left = syn-serv    # "normal"
        right = syn-serv   # "wrapping"
end
section: options
        keystroke(control+super+right) = switchInDirection(right) # Switch screens on keypress 
        keystroke(control+super+left) = switchInDirection(left)
end
```

### AltGr key

The following screen config allows the mapping for Alt to AltGr. Although this may not work, see [#1](#1)
```
section: screens
	client1:
		altgr = alt          # mapping to fix AltGr key not working on windows clients (e.g. @-Symbol etc.).
		                     # See reference [1]
end
```

See also: the man page for synergys

<span id="1">[1]</span> [Synergy Issue Tracker Bug #4411 - AltGr not sent to client from server](https://github.com/synergy/synergy/issues/4411)

Tips/Notes
----------

The GUI actually creates a text-based config behind the scenes.

