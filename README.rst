Introduction
============

.. image:: https://readthedocs.org/projects/adafruit-circuitpython-hcsr04/badge/?version=latest
    :target: https://circuitpython.readthedocs.io/projects/hcsr04/en/latest/
    :alt: Documentation Status

.. image:: https://img.shields.io/discord/327254708534116352.svg
    :target: https://discord.gg/nBQh6qu
    :alt: Discord

.. image:: https://travis-ci.com/adafruit/Adafruit_CircuitPython_HCSR04.svg?branch=master
    :target: https://travis-ci.com/adafruit/Adafruit_CircuitPython_HCSR04
    :alt: Build Status

.. image:: https://github.com/adafruit/Adafruit_CircuitPython_HCSR04/blob/master/docs/hcsr04.jpg
    :width: 300px
    :alt: HCSR04

The HC-SR04 is an inexpensive solution for measuring distances using microcontrollers. This library provides a simple
driver for controlling these sensors from CircuitPython.

Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://github.com/adafruit/Adafruit_CircuitPython_Bundle>`_.

Usage Example
=============

.. warning::

    The HC-SR04 uses 5V logic, so you will have to use a `level shifter
    <https://www.adafruit.com/product/2653?q=level%20shifter&>`_ between it
    and your CircuitPython board (which uses 3.3V logic).

.. note::

    If you want to use an HC-SR04 with `MicroPython <http://micropython.org/>`_, I recommend checking out `this library
    <https://github.com/andrey-git/micropython-hcsr04>`_.

You'll need to dedicate two pins to communicating with the HC-SR04. The sensor communicates in a very rudimentary
manner, so it doesn't matter which pins you choose, as long as they're digital IO pins (pins that start with "``D``"
are digital).

There are two ways of instantiating a :class:`~hcsr04.HCSR04` object: with or without using a context manager.

.. note::

    It is technically possible to communicate with the HC-SR04 using only one wire since the trigger and echo signals
    aren't ever active at the same time. Once I have a chance to determine a safe way to do this, I plan to add this as
    a feature to the library.

.. seealso::

    `Adafruit's guide on Lifetime and ContextManagers <https://circuitpython.readthedocs.io/en/latest/docs/design_guide.html#lifetime-and-contextmanagers>`_
        Gives more info on using context managers with CircuitPython drivers.

    :any:`board`
        A list of pins available on your device. To view this list, first `get a REPL
        <http://circuitpython.readthedocs.io/en/latest/docs/pyboard/tutorial/repl.html>`_ (the guide linked was written
        for the pyboard, but it still works), then input the following:

        ::

            import board
            dir(board)

Without a Context Manager
-------------------------

In the example below, we create the :class:`~hcsr04.HCSR04` object directly, get the distance every 2 seconds, then
de-initialize the device.

::

    from hcsr04 import HCSR04
    sonar = HCSR04(trig, echo)
    try:
        while True:
            print(sonar.dist_cm())
            sleep(2)
    except KeyboardInterrupt:
        pass
    sonar.deinit()


With a Context Manager
----------------------

In the example below, we use a context manager (the :any:`with <with>` statement) to create the :class:`~hcsr04.HCSR04`
instance, again get the distance every 2 seconds, but then the context manager handles de-initializing the device for
us.

::

    from hcsr04 import HCSR04
    with HCSR04(trig, echo) as sonar:
        try:
            while True:
                print(sonar.dist_cm())
                sleep(2)
        except KeyboardInterrupt:
            pass


Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_HCSR04/blob/master/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.

Building locally
================

Zip release files
-----------------

To build this library locally you'll need to install the
`circuitpython-build-tools <https://github.com/adafruit/circuitpython-build-tools>`_ package.

.. code-block:: shell

    python3 -m venv .env
    source .env/bin/activate
    pip install circuitpython-build-tools

Once installed, make sure you are in the virtual environment:

.. code-block:: shell

    source .env/bin/activate

Then run the build:

.. code-block:: shell

    circuitpython-build-bundles --filename_prefix adafruit-circuitpython-hcsr04 --library_location .

Sphinx documentation
-----------------------

Sphinx is used to build the documentation based on rST files and comments in the code. First,
install dependencies (feel free to reuse the virtual environment from above):

.. code-block:: shell

    python3 -m venv .env
    source .env/bin/activate
    pip install Sphinx sphinx-rtd-theme

Now, once you have the virtual environment activated:

.. code-block:: shell

    cd docs
    sphinx-build -E -W -b html . _build/html

This will output the documentation to ``docs/_build/html``. Open the index.html in your browser to
view them. It will also (due to -W) error out on any warning like Travis will. This is a good way to
locally verify it will pass.
