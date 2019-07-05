xcwd - X current working directory
==================================
xcwd is a simple tool that prints the current working directory of the
currently focused window.

The main goal is to launch applications directly into the same directory as the
focused applications. This is especially useful to open a new terminal or a
file explorer.

Disclaimer
----------
This program is basically a hack, but it works well with my setup and I hope
it will work for you as well :)

This script **can't** retrieve the working directory of a "single instance
application" nor terminal multiplexer, e.g.: 
  - tmux, screen
  - lilyterm
  - konsole
  - ~urxvtc with urxvtd~
  - applications with tabs

The application works with the following terminals:
  - urxvt
  - xterm
  - gnome terminal
  - terminology
  - st

How it works
------------
  - Get the handle of the focused window;
  - Try to get the PID of the program using the window's attributes:
     - If `_NET_WM_PID` is set, xcwd just reads the value;
     - Otherwise it reads the `_NET_WM_CLASS` and compares it to the name of
       all the running processes;
  - Find the deepest child process or stop at the first matching name pattern;
  - Print the working directory of this process to stdout.

If one of these steps fail, xcwd prints the content of the `$HOME` variable.

Requirements
------------
  - Linux, FreeBSD or OpenBSD
  - libX11-dev

Installation
------------
* Clone this repository or [download as ZIP](https://github.com/schischi/xcwd/archive/master.zip)
* `make`
* `make install`
Use `gmake` on OpenBSD.

Running xwcd
------------
Simply invoke the 'xcwd' command.

Examples:
* ``urxvt -cd "`xcwd urxvt`" ``
* ``xterm -e "cd `xcwd zsh` && /bin/zsh"``
* ``gnome-terminal --working-directory="`xcwd zsh bash urxvt`"``
* ``pcmanfm "`xcwd`" ``

i3 Configuration
----------------
* bindsym $mod+Shift+Return exec ``urxvt -cd "`xcwd`" ``
* bindsym $mod+Shift+Return exec ``cd "$(xcwd)" && exec xterm``
* bindsym $mod+Shift+Return exec ``gnome-terminal --working-directory="`xcwd`"``
* bindsym $mod+p            exec ``pcmanfm "`xcwd`"``

Match pattern rules
-------------------
This xcwd version allow user to retrieve working directory when a script is execute in a shell
This hack increase compute cost. When searching for deepest child, it compare it's name to one of the given name
To use the features, juste add arguments to xcwd with exact name.
example for urxvt:
``xcwd zsh bash``
