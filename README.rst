CircuitPython HC-SR04 Driver
============================

|docs| |version| |ci| |license_type|

.. image:: hcsr04.jpg
    :width: 300px

The HC-SR04 is an inexpensive solution for measuring distances using microcontrollers. This library provides a simple
driver for controlling these sensors from `CircuitPython`_, Adafruit's port of `MicroPython <http://micropython.org/>`_.


Installation
------------

This driver depends on `CircuitPython <https://github.com/adafruit/circuitpython>`_ and is designed for use with an
HC-SR04 ultrasonic range sensor. You'll also need to ensure all dependencies are available on the CircuitPython
filesystem. This is easily achieved by downloading
`the Adafruit library and driver bundle <https://github.com/adafruit/Adafruit_CircuitPython_Bundle>`_.

.. warning::

    The HC-SR04 uses 5V logic, so you will have to use a `level shifter
    <https://www.adafruit.com/product/2653?q=level%20shifter&>`_ between it
    and your CircuitPython board (which uses 3.3V logic).

.. note::

    If you want to use an HC-SR04 with `MicroPython <http://micropython.org/>`_, I recommend checking out `this library
    <https://github.com/andrey-git/micropython-hcsr04>`_.


Quick Start
-----------

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
^^^^^^^^^^^^^^^^^^^^^^^^^

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
^^^^^^^^^^^^^^^^^^^^^^

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


API Reference
-------------

.. toctree::
   :maxdepth: 2

   api


Contributing
------------

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_HCSR04/blob/master/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.


License
-------

This project is licensed under the `MIT License <https://github.com/mmabey/CircuitPython_HCSR04/blob/master/LICENSE>`_.


.. |docs| image:: https://readthedocs.org/projects/adafruit-soundboard/badge/
    :alt: Documentation Status
    :target: `Read the Docs`_

.. |version| image:: https://img.shields.io/github/release/mmabey/CircuitPython_HCSR04/all.svg
    :alt: Release Version
    :target: https://github.com/mmabey/CircuitPython_HCSR04

.. |ci| image:: https://travis-ci.org/mmabey/CircuitPython_HCSR04.svg
    :alt: CI Build Status
    :target: https://travis-ci.org/mmabey/CircuitPython_HCSR04

.. |license_type| image:: https://img.shields.io/github/license/mmabey/CircuitPython_HCSR04.svg
    :alt: License: MIT
    :target: `GitHub`_

.. _GitHub: https://github.com/mmabey/CircuitPython_HCSR04

.. _CircuitPython: https://github.com/adafruit/circuitpython

.. _Read the Docs: http://circuitpython-hcsr04.readthedocs.io/
