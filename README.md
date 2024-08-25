Pymissari
=========

Pymissari is a  cross-platform GUI automation Python module for human beings. Used to programmatically control the mouse & keyboard.

`pip install Pymissari`

Full documentation available at https://Pymissari.readthedocs.org

Simplified Chinese documentation available at https://github.com/asweigart/Pymissari/blob/master/docs/simplified-chinese.ipynb

Source code available at https://github.com/asweigart/Pymissari

If you need help installing Python, visit https://installpython3.com/

Dependencies
============

Pymissari supports Python 2 and 3. If you are installing Pymissari from PyPI using pip:

Windows has no dependencies. The Win32 extensions do not need to be installed.

macOS needs the pyobjc-core and pyobjc module installed (in that order).

Linux needs the python3-xlib (or python-xlib for Python 2) module installed.

Pillow needs to be installed, and on Linux you may need to install additional libraries to make sure Pillow's PNG/JPEG works correctly. See:

    https://stackoverflow.com/questions/7648200/pip-install-pil-e-tickets-1-no-jpeg-png-support

    http://ubuntuforums.org/showthread.php?t=1751455

If you want to do development and contribute to Pymissari, you will need to install these modules from PyPI:

* pyscreeze
* pymsgbox
* pytweening

Example Usage
=============

Keyboard and Mouse Control
--------------------------

The x, y coordinates used by Pymissari has the 0, 0 origin coordinates in the top left corner of the screen. The x coordinates increase going to the right (just as in mathematics) but the y coordinates increase going down (the opposite of mathematics). On a screen that is 1920 x 1080 pixels in size, coordinates 0, 0 are for the top left while 1919, 1079 is for the bottom right.

Currently, Pymissari only works on the primary monitor. Pymissari isn't reliable for the screen of a second monitor (the mouse functions may or may not work on multi-monitor setups depending on your operating system and version).

All keyboard presses done by Pymissari are sent to the window that currently has focus, as if you had pressed the physical keyboard key.

```python
    >>> import Pymissari
    >>> screenWidth, screenHeight = Pymissari.size() # Returns two integers, the width and height of the screen. (The primary monitor, in multi-monitor setups.)
    >>> currentMouseX, currentMouseY = Pymissari.position() # Returns two integers, the x and y of the mouse cursor's current position.
    >>> Pymissari.moveTo(100, 150) # Move the mouse to the x, y coordinates 100, 150.
    >>> Pymissari.click() # Click the mouse at its current location.
    >>> Pymissari.click(200, 220) # Click the mouse at the x, y coordinates 200, 220.
    >>> Pymissari.move(None, 10)  # Move mouse 10 pixels down, that is, move the mouse relative to its current position.
    >>> Pymissari.doubleClick() # Double click the mouse at the
    >>> Pymissari.moveTo(500, 500, duration=2, tween=Pymissari.easeInOutQuad) # Use tweening/easing function to move mouse over 2 seconds.
    >>> Pymissari.write('Hello world!', interval=0.25)  # Type with quarter-second pause in between each key.
    >>> Pymissari.press('esc') # Simulate pressing the Escape key.
    >>> Pymissari.keyDown('shift')
    >>> Pymissari.write(['left', 'left', 'left', 'left', 'left', 'left'])
    >>> Pymissari.keyUp('shift')
    >>> Pymissari.hotkey('ctrl', 'c')
```

Display Message Boxes
---------------------
```python
    >>> import Pymissari
    >>> Pymissari.alert('This is an alert box.')
    'OK'
    >>> Pymissari.confirm('Shall I proceed?')
    'Cancel'
    >>> Pymissari.confirm('Enter option.', buttons=['A', 'B', 'C'])
    'B'
    >>> Pymissari.prompt('What is your name?')
    'Al'
    >>> Pymissari.password('Enter password (text will be hidden)')
    'swordfish'
```

Screenshot Functions
--------------------

(Pymissari uses Pillow for image-related features.)
```python
    >>> import Pymissari
    >>> im1 = Pymissari.screenshot()
    >>> im1.save('my_screenshot.png')
    >>> im2 = Pymissari.screenshot('my_screenshot2.png')
```
You can also locate where an image is on the screen:
```python
    >>> import Pymissari
    >>> button7location = Pymissari.locateOnScreen('button.png') # returns (left, top, width, height) of matching region
    >>> button7location
    (1416, 562, 50, 41)
    >>> buttonx, buttony = Pymissari.center(button7location)
    >>> buttonx, buttony
    (1441, 582)
    >>> Pymissari.click(buttonx, buttony)  # clicks the center of where the button was found
```
The locateCenterOnScreen() function returns the center of this match region:
```python
    >>> import Pymissari
    >>> buttonx, buttony = Pymissari.locateCenterOnScreen('button.png') # returns (x, y) of matching region
    >>> buttonx, buttony
    (1441, 582)
    >>> Pymissari.click(buttonx, buttony)  # clicks the center of where the button was found
```

How Does Pymissari Work?
========================

The three major operating systems (Windows, macOS, and Linux) each have different ways to programmatically control the mouse and keyboard. This can often involve confusing, obscure, and deeply technical details. The job of Pymissari is to hide all of this complexity behind a simple API.

* On Windows, Pymissari accesses the Windows API (also called the WinAPI or win32 API) through the built-in `ctypes` module. The `nicewin` module at https://github.com/asweigart/nicewin provides a demonstration for how Windows API calls can be made through Python.

* On macOS, Pymissari uses the `rubicon-objc` module to access the Cocoa API.

* On Linux, Pymissari uses the `Xlib` module to access the X11 or X Window System.

