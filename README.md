calculon
========

A terminal-based programmer's calculator
----------------------------------------

I haven't found many decent programmer's calculators for Mac and I spend a fair bit of time copying and pasting between Calculator.app and a Python REPL, so I figured I'd have a go at writing a quick terminal-based calculator in Python. The result is Calculon.

Calculon is a programmer's calculator based on an embedded Python REPL. It's split into two components - the display and the REPL - each of which are run in a separate terminal. There are two options for the REPL - either the embedded Python REPL (based on the Python `code` module, or an instance of `bpython`.

Here is Calculon running on top of `bpython` in two panes of an iTerm window:

[![calculon example](http://i.imgur.com/F5BJYAu.png)](#example)

And in a wider window:

[![calculon example2](http://i.imgur.com/aqb6a1u.png)](#example2)

And here is Calculon running with the embedded REPL, again using iTerm panes:

[![calculon example3](http://i.imgur.com/9CaINxg.png)](#example3)

Dependencies
------------

Calculon requires the `Pyro4`, `blessings` and `rl` modules. They will be automatically installed by the `setup.py` script.

To use `bpython` as the REPL you will obviously have to have `bpython` installed.

Installation
------------

A standard `setuptools` script is included.

    # python setup.py install


Configuration
-------------

An example config (`example.cfg`) is included with the source. Copy it to `~/.calculon/config` and edit it if you like, otherwise the defaults in the `defaults.cfg` will be used.

Usage
-----

To run the display:

	$ calculon display

To run the embedded REPL:

	$ calculon console

Or, to connect to the display from within a `bpython` instance:

	$ bpython
	>>> import calculon.load

From here, any Python code entered into the REPL that results in a numeric value will be rendered in the display. For example:

	>>> 1234 + 1234
	2468

2468 will be rendered in the display.

Calculon adds some hackery to the REPL for watching variables. Calling `watch <expr>` will add the given expression to a list of expressions that are tracked and updated every time they change. For example:

    >>> watch a
    >>> watch b
    >>> watch a + b

Now when these variables are updated:

	>>> a = 1234
	>>> b = 1234

Their values will be tracked and the expressions reevaluated. Expressions can be removed from this display with the `unwatch` keyword:

	>>> unwatch 0

Where 0 is the ID displayed at the end (or beginning, when right aligned) of the line.

Calculon now has support to connect to [voltron](https://github.com/snarez/voltron) and inspect register state. If you have the most recent version of calculon and voltron, and voltron is running, calculon will connect to it at startup. Calculon can manually connect and disconnect from voltron as follows:

	>>> V.connect()
	Connected to voltron
	>>> V.disconnect()
	Disconnected from voltron

When connected to voltron, calculon can inspect registers:

	>>> V.rip
	4294971201

Or memory:

    >>> V[V.rbp]
    'x'
    >>> V[V.rbp:V.rbp + 32]
    'x\xee\xbf_\xff\x7f\x00\x00\xfd\xf5\xad\x85\xff\x7f\x00\x00'


Credits
-------
[richo](https://github.com/richo) deserves many beers for his efforts