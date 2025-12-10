Hi Azeem here!

- I Re-added the html entity filtering code from 0.7.0 and added a thorough list of named entities in tickr_html_entities.h . Feel free to add more and recompile.

- I also re-added 0 padding on date and hour so the length is consistent. 


context: I use Tickr in a broadcast as a video source in OBS with RSS feeds.

html entity filtering handles most things libxml2 was missing (named entities).

the change in length of the window from 9:50 AM to 10:00 AM because of the extra integer in the hour would force the tickr offscreen. By forcing 0 padding in the date it will say Wed Dec 01 instead of Wed Dec 1, and 09:30 PM instead 9:30 PM etc. This gives you a consistent ticker year round for a stable broadcast layout consistency... (I like to line up my logo with the "|" separating the clock from the ticker feed.)

there's probably a better way to update to newer xml parser and use strftime for the clock for uniformity and aesthetic consistency, but I'm sure the original author knows better. I'm just a guy sharing a fix that works that I compiled and use on my own machine.

The original author's site https://www.open-tickr.net/

how to build: 

`https://www.open-tickr.net/help.php`



## From the OG
Known issues in last stable version (0.7.1)
- Still no Win64 binaries available at the moment.

- The program still only implements HTTP *basic* authentication (although nobody ever asked for HTTP *digest* authentication support.)

("Workflow" courtesy of xkcd.com.)


 Building from source HOWTO
=== To build from source on Linux ===

Required packages are GTK+, Libxml2, GnuTLS and Libfribidi (development ones.)

- Download tickr-0.7.1.tar.gz and extract somewhere in your home dir.
- Open a terminal and cd to the tickr-0.7.1 dir you just created.
- Type in terminal:

On Debian/Ubuntu:

```
    sudo apt-get install libgtk2.0-dev libxml2-dev libgnutls28-dev libfribidi-dev
    ./configure
    make
    sudo make install
    (make clean)
```

On Fedora:

```
    su
    yum install gtk2-devel libxml2-devel gnutls-devel fribidi-devel
    exit
    ./configure
    make
    su
    make install
    exit
    (make clean)
```

This will install tickr (binary file) in /usr/bin/ and icon files (tickr-icon.*) and png files in /usr/share/tickr/pixmaps/ (of course the debian package install these files in the same locations.)

You may also copy tickr_url_list in /home/USER_NAME/.tickr/ (it's a sample list of RSS feeds.)


=== To build from source on Windows ===

You will have to install MinGW and GTK development stuff (headers and libs.) You will also need inno setup and reshacker (reshacker.exe must be installed in /usr/local/ResHack/.)

If you want to use autotools, you will have to hack a little bit and re-create your own Makefile.am and configure.ac. Those provided are ok for ***Linux only*** at the moment.

So, instead of autotools, I use a script (build-on-win32) which works fine for me on XP. You will have to adapt it to your build system, at least replacing "manutm" by your user name (I'm working on improving this script.)

You must also download gtk2-win32-runtime-bin.tar.gz (the GTK stack runtime which includes a patched version of GLib), and dlls.tar.gz (which contains libxml2, libgnutls and libfribidi dlls.) Get them from the download page, and extract them.

(About GLib, you may too get glib sources, apply the patch, compile it yourself then add it to the GTK runtime stack you will have to build. Visit www.gtk.org for more info.)

Copy gtk-win32-full-runtime under tickr-0.7.1/ and dlls under tickr-0.7.1/win32_install_stuff/, then run:

    ./build-on-win32

It will build the win32 installer.

Please, just let me know in case of issues and I'll try to help fix them.


 A few tips to get you started

- To open the main menu, right-click inside ticker area.

- You can import feed subscriptions with 'File > Import Feed List (OPML)', for instance your Google Reader subscriptions.

- To open a link in your browser, left-click on text.

- Use mouse wheel to either adjust the ticker scrolling speed or open the 'Selected Feed Picker' window to quickly move between selected feeds (if set, Ctrl + mouse wheel scrolling applies to other value.), This behaviour is set in the 'Preferences' window as 'Mouse wheel acts on'.

- Basically, you will use 'File > Feed Organizer (RSS|Atom)' to pick a feed from a list, subscribe to a new one, manage your feed list, and 'Edit > Preferences' to tweak the ticker appearance as well as other settings.

- 'Window - always on top' -> check this if you want the ticker to always stay above your browser (and any other windows.)

- 'Window - decorated' -> if you want a 'draggable' ticker.


There is a sample list of already subscribed feeds that you can use.


***ONLY*** in case you're using the ticker inside a ***decorated*** window, you can use these keyboard shortcuts:

- ctrl+R to open the 'RSS Feed Picker' window (to choose a feed or to subscribe to a new one.)

- ctrl+T to open a text file.

- ctrl+I to import (and merge) an URL list from an OPML file.

- ctrl+E to export the URL list to an OPML file.

- ctrl+P to open the 'Resource Properties' window.

- ctrl+Q to quit.

- ctrl+S to open the 'Preferences' (Settings) window.

- ctrl+B to open the link displayed inside the ticker (in the middle.)

- ctrl+J to play the feed / ctrl+K to pause / ctrl+L to reload.

- ctrl+U (speed Up) / ctrl+D (speed Down) to adjust scrolling speed on the fly.

- F1 to open the 'Quick Help' window.

- ctrl+H to launch the 'Online Help' (this very page.)

- ctrl+A to open the 'About' window and the License window.

FAQ
Q: Is there any way to reduce the cpu load ?

A: The cpu load comes mainly from fully redrawing the ticker area at regular time intervals, so to reduce this load, you may adjust some settings: try increasing both delay and shift size (eg delay = 32 and shift size = 4.) But scrolling will get less smooth.
You may also decrease ticker surface (ie width and height or fontsize.)

Q: When starting the program with no internet connection a black square appears on the screen. What is that?

A: This sometimes happens just before the ticker start showing, when (re)computing ticker dimensions. And it may become more noticeable when starting with no connection as the ticker waits until it can connect.

Q: On Window, I am not entirely sure how to edit the feed list. When I reinstall I have the following structure: C:\Program Files\Tickr\tickr_url_list. In the tickr_url_list I have a load of other feeds and no mention of my custom feed. Therefore I do not know where the app is reading it's list from. What would be the default directory structure where I could find the file that needs editing?

A: 'tickr_url_list' under 'C:\Program Files\Tickr\' is a sample URL list.
Your feeds (also named 'tickr_url_list') are under: C:\Documents and Settings\USER_NAME\Application Data\Tickr\ (or something very similar.)
On Linux, they will be under: /home/USER_NAME/.tickr/

Q: I'm running Windows Vista with SP2 and I got problem (feed format error) with: http://news.google.co.uk/news?pz=1&jfkl=true&cf=all&ned=uk&hl=en&output=rss

A: When checked with https://validator.w3.org/feed/, the feed doesn't validate.
Some feeds are not fully correctly formated from time to time. Maybe format checking in the program should be modified so that it's more fault-tolerant.

Q: Also, what is 'Reload Delay'? I've had a few problems in which it couldn't load the rss feed, does that command try to reload it again in x minutes?

A: No, if the feed can't be loaded, the program moves to the next one in the selection (in multiple selections mode) or returns an error message (in single selection mode.)
Feeds may include a 'TTL' element which specify after how many minutes they should be refreshed (or reloaded), even if they were already successfully loaded. This is a way of always having up-to-date content. For feeds without TTL element, the 'reload delay' is used instead.

Q: I type wrong number of pixels and now my bar is under my top system bar. I try reinstall, shortcut Ctrl+S but it didn't help. So my question is - where is setting file because I couldn't locate it.

A: On Linux, it's in a hidden directory: /home/USER_NAME/.tickr/tickr_conf
On Windows, it's under the 'Application Data' folder so it should be something like: C:\Document and Settings\USER_NAME\Application Data\Tickr\tickr_conf

Q: Do you think you will be able implement "independent color alpha channel (transparency) for text / background" for the windows version?

A: I'm trying to solve that. ;)

Q: I am not sure how to do the feed export. The online help says ctrl+E when in using decorated window, I have clicked decorated window in the preferences and am pressing ctrl+E but this doesn't seem to do anything. I assume I am missing something obvious but can't see what.

A: Just right-click on the ticker, then 'File' > 'Export OPML File'.

Q: When i try to enter new URL under the RSS Feed Picker, i got the error message "Invalid URL: http://www.rthk.org.hk/rthk/news/rss/c_expressnews.xml". Since the above URL is encoded in chinese character big5 format, will there be any solution in supporting URL in chinese characters?

A: URLs are always 'translated' from local encoding to UTF-8 so if you get this error message, it really means the URL either ***is not*** valid or ***was not*** valid at the time.
Rendering of big5 encoded text is available in the ticker, although on Windows, you must make sure you selected the right font (in the preference window.) I tried with the URL you provided and with 'Arial Unicode MS' and it worked perfectly.

Q: I use it in Ubuntu 10.10, but I have a problem: I have to re-start Tickr (News RSS Ticker) every time I start my session on Ubuntu.

A: To start the ticker with every session in Ubuntu, you can use 'System > Preferences > Startup Applications'. Just add the ticker as another startup program. The command should be: '/usr/bin/tickr'.

Q: I cant see the ticker cos i changed the preferences. doh! how do i reset the prefs file to default in ubuntu 10.10?

A: The preference file is in a hidden dir in your home dir: /home/USER_NAME/.tickr/tickr_conf. Just remove it...

Q: What should I type in a terminal to install Tickr (News RSS Ticker) in Ubuntu Natty?

A: The following answer applies to Ubuntu series from Maverick to Precise.
On Precise: sudo apt-get install tickr
On earlier series (from Maverick to Oneiric), you will have first to enable backports (unsupported updates) with Synaptic or Update Manager > Settings > Software Sources. Then:
sudo apt-get update
sudo apt-get install tickr

Q: How can I simultaneously run several instances of the program with different settings?

A: you can set up 2 separate tickers with different instance-ids. To achieve that, you must use the command line (or a little script):
On Linux:
'/usr/bin/tickr -instance-id=1 -win_y=200 & /usr/bin/tickr -instance-id=2 -win_y=250'
You can then set up separately feeds and preferences for each instance. After that, use a launcher that will run the script.
On Windows, you should use a batch file instead and a different path.
(Note: This question has also been discussed on askubuntu.com.)











1 - BUILDING TICKR FROM SOURCES

on Linux
--------

Required packages are GTK+2, Libxml2 and GNUTls (development ones.) Then do:

./configure
make
(sudo) make install
(make clean)


on Windows
----------

You will have to install MinGW and GTK development stuff (headers and libs.) You will also need inno setup and reshacker (reshacker.exe must be installed in /usr/local/ResHack/.)

If you want to use autotools, you will have to hack a little bit and re-create your own Makefile.am and configure.ac. Those provided are ok for ***Linux only*** at the moment.

So, instead of autotools, I use a script (build-on-win32) which works fine for me on XP. You will have to adapt it to your build system, at least replacing "manutm" by your user name (I'm working on improving this script.)

You must also download gtk2-win32-runtime-bin.tar.gz (from www.open-tickr.net), the GTK stack runtime which includes a patched version of glib. (Of course, you may too get glib-2.26.0 sources, apply the patch, compile it yourself then add it to the GTK runtime stack you will have to build. Visit www.gtk.org for more info.)

Copy gtk-win32-full-runtime under tickr-<version_num> and run:

`./build-on-win32`

(it will build the win32 installer.)


2 - INSTALL DIRS DEFINED IN

- tickr.desktop
- debian/install
- debian/menu
- src/tickr/tickr.h
- src/tickr/Makefile.am


3 - APPLICATION NAME

source/binary package name and command

previous name:			news
last stable version:		0.5.2

new name:			tickr
first released version:		0.5.3

in src/tickr: all source files have now been renamed:
news_*.c/h -> tickr_*/c/h

app name and dirs are all (?) defined in tickr.h (at least they should be)


4 - WIDGETS PACKING

see: doc/widgets_packing
