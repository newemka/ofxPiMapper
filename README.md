![Pi Mapper Logo](piMapperLogo.jpg)

ofxPiMapper
===========

This project is an attempt to create optimized version of an openFrameworks projection mapping addon for the Raspberry Pi. It is also my master thesis project at Aalto University, School of Arts, Design and Architecture (Helsinki Media Lab).

ofxPiMapper is mainly for people who want to use the Raspberry Pi as a cheaper way to create standalone mapping installations. Let's say someone has 10 generative mapping pieces and gets an offer to exhibit them in an art gallery, museum or any other kind of place. 10 expensive computers? No! PiMapper is here to save you!

ofxPiMapper features FBO sources that allow you to create generative openFrameworks patches to be used with ofxPiMapper. Extend the `FboSource` class, override `setup()`, `update()` and `draw()` methods and then create an instance of your custom source to add it to your ofxPiMapper object.

```
ofxPiMapper* mapper = new ofxPiMapper();
CustomSource* source = new CustomSource();
mapper.addFboSource(source);
mapper.setup(); // Setup must be run after adding the FBO source
```

Since version 0.2.0 ofxPiMapper supports video sources and since version 0.2.3 ofxPiMapper supports undo.

Development
-----------

As the projects gets a bit more popular, I see that people want to add missing features. I have a whole bunch of features that I want to add in future releases, but right now I'm trying to understand how to keep it more or less organized.

Currently I have decided to use [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/) by [Vincent Driessen](https://twitter.com/nvie), so read this article and I do not doubt that it will help you with other Git related projects.

I'm trying to organize the project by adding future release version milestones and assigning specific issues as TODO items to them. If you notice something strange or see that there is something that can be done in a better way, don't hesitate and add an issue.

As of Release 0.2.0 some refractoring has been done (thanks [aspeteRakete](https://github.com/aspeteRakete)), namespaces have been added and we are thinking about introducing a code style for this project. Still improving on the overal structure.

Licence
-------
ofxPiMapper is distributed under the [MIT License](https://en.wikipedia.org/wiki/MIT_License).

Installation
------------
If you don't have a clue on how to set up your Pi - go to the [Raspberry Pi website](http://www.raspberrypi.org/downloads/) and follow the instructions there. I have been using Raspbian so far as well as [Satellite CCRMA](https://ccrma.stanford.edu/~eberdahl/satellite/) without problems.

Another thing you need is a working [openFrameworks](http://openframeworks.cc) installation. There are 2 openFrameworks setup guides now as there are 2 versions of Raspberry Pi:

 - [Raspberry Pi 1 oF Setup Guide](http://openframeworks.cc/setup/raspberrypi/)  
 - [Raspberry Pi 2 oF Setup Guide](http://forum.openframeworks.cc/t/raspberry-pi-2-setup-guide/18690)  

Clone this (the ofxPiMapper) repository into your `openFrameworks/addons` folder. If your openFrameworks installation on the Pi resides in the home directory, you have to type the following in the terminal:

```bash
cd ~/openFrameworks/addons
git clone https://github.com/kr15h/ofxPiMapper.git
```

Dependencies
------------
Before moving on, make sure that you have all the dependencies installed. 

 - ofxGui (available in oF by default)
 - ofxXmlSettings (available in oF by default)
 - [ofxIO](https://github.com/bakercp/ofxIO)
 - [ofxOMXPlayer](https://github.com/jvcleave/ofxOMXPlayer)

To install dependencies, `cd` into `openFrameworks/addons` directory and execute the following:

```bash
git clone https://github.com/jvcleave/ofxOMXPlayer.git && git clone https://github.com/bakercp/ofxIO.git
```

And you are good to go!


First Test
----------
To test the addon, you will have to compile and run it:

```bash
cd ~/openFrameworks/addons/ofxPiMapper/example
make && make run
```

After it compiles, run it with either `make run` or `./bin/example`. You can exit the application by pressing `ESC` or `Ctrl + C`.

It will take a while first (not so long on the RPi 2), but once it runs, press 1, 2, 3 and 4 keys to switch between modes of the software. Switch to mode 3 at first to select a surface. Afterwards you will be able to edit the texture mapping of it in mode 2 and choose a source in mode 4. Mode 1 is the presentation mode. It is activated on start by default.

Usage
-----
Right now a keyboard and a mouse has to be used in order to do the mapping with ofxPiMapper.

###Modes

PiMapper has 4 modes:

1. Presentation mode
2. Texture mapping mode
3. Surface editing mode
4. Source assignment mode

You can access these modes by pressing 1, 2, 3 or 4 respectively.

####Presentation mode

This mode is activated once the application starts up. It does not show anything else except the final projection mapping as it was saved previously.

####Texture mapping mode

In this mode you can adjust the texture coordinates of the surface you have selected in the surface editing mode.

####Surface editing mode

Here you can select, move and distort the surfaces you have created.

####Source assignment mode

After you select a surface in surface editing mode, activate this mode to be able to choose a source for the surface. Afterwards you might want to go to the texture mapping mode to adjust texture coordinates.

###Other shortcuts

These other shortcuts that you can use while using the example app. Remember that you can assign your own by editing the app.cpp file.

Key | Function
:--- | :---
i | Show info
t | Add triangle surface
q | Add quad surface
f | Toggle fullscreen
s | Save composition
z | Undo
BACKSPACE | Delete surface


Compatibility
-------------
Tested with 0.8.4 (OS X and Raspbian)

Known issues
------------
Keyboard and mouse input is being lost sometimes on the Raspberry Pi. Not sure how and why. Probably because of bad drivers and it seems that Raspberry Pi keyboard and mouse code in openFrameworks is not quite ready yet. Some claim that the following commands solves the issue:

```bash
sudo apt-get update && sudo apt-get dist-upgrade  
sudo rpi-update
```

[ofxOMXPlayer](https://github.com/jvcleave/ofxOMXPlayer) has an issue, it throws an error when compiling:

```
fatal error: libavcodec/opt.h: No such file or directory
```

To fix that, create a file `opt.h` in `addons/ofxOMXPlayer/libs/ffmpeg/include/libavcodec/` with the following contents: 

**opt.h**

```
#ifndef AVCODEC_OPT_H
#define AVCODEC_OPT_H
#include "libavcodec/version.h"
#if FF_API_OPT_H
#include "libavutil/opt.h"
#endif
#endif // AVCODEC_OPT_H
```
   
More about this issue [here](https://github.com/jvcleave/ofxOMXPlayer/issues/34). 

Version history
---------------
###TODO
A short wishlist for the next releases:

 - Refined directoryWatcher mechanism, source lists should react on added and removed files
 - OSC remote control module
 - Webserver with simple interface for remote mapping
 - Syphon source for Mac
 - Spout source for Win
 - Streaming source for RPi (fb sharing, network streams...)
 - Even better code structure
 
### Version 0.2.4 (2015-05-31)
 - Fix RPi compile bug
 
### Version 0.2.3 (2015-05-22)
 - Add undo functionality

### Version 0.2.2 (2014-11-17): 
 - Remove perspective warping in favour of doing mesh warping first and then adding proper perspective warping as a wrapper of the mesh warp
 - Fixed issue 24 (selected surface FBO source not checked in the sources editor view)
 - Fix OMX player workaround instructions

### Version 0.2.1 (2014-11-05):
 - Added single instance feature. Now you can use ofxPiMapper as single object for your project.
 - Added FBO source. You can create a custom openFrameworks application for piMapper by extending the FboSource class. Add/register your custom object as source in piMapper and you will be able to select it in the source editor. Source is saved and loaded from the settings as well.
 - Fixed issue #15
 - Added -f (fullscreen) flag for the Raspberry Pi version. Use `./yourApp -f` to launch it fullscreen on start.
 - Added XCode project file to example.

### Version 0.2.0 (2014-10-18):
 - Added logo (thanks [Irina Spicaka](http://irina.spicaka.info/))
 - Added perspective warping for quad surfaces (thanks [aspeteRakete](https://github.com/aspeteRakete))
 - Added namespaces
 - Replaced ofxUI with ofxGui
 - Added media server (thanks [aspeteRakete](https://github.com/aspeteRakete))
 - Added source types and reworked all relevant classes to support them instead of ofTexture directly
 - Added video source (based on [ofxOMXPlayer](https://github.com/jvcleave/ofxOMXPlayer))

### Version 0.1.4 (2014-07-10):
 - Added fbo texture example
 - Replaced the main example with the fbo texture one
 - Added simple quad surface (thanks [sebllll](https://github.com/sebllll))
 - Fixed a couple of bugs
 - Introduced new branching model
 - Added joint snapping
 - Improved README

### Version 0.1.3 (2014-05-16):
 - Added sources view (for selected surface)
 - Added possibility to select image sources
 - Save and load surfaces with image sources
 - Joint and surface movement using arrow keys
 - Added fullscreen toggle
 - Adjusted the example to work with new changes

### Version 0.1.2 (2014-05-12):
Restructured GUI mechanism for flexibility later.
 - Added possibility to add multiple surfaces
 - Add GUI for surface manager instead per surface
 - Ability to select and drag whole surfaces as well as select/deselect surfaces

### Version 0.1.1 (2014-05-10):
Added simple GUI with joints. Updated the example application so that it allows the user to do texture mapping, projection mapping and enter presentation mode.

### Version 0.1 (2014-05-09):
First usable version. For now there is a triangle surface class that you can use to map an ofTexture object. 
 - Added triangle surface
 - Added simple example
