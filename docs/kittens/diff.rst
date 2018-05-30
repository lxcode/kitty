kitty-diff - A side-by-side diff tool with syntax highlighting and images
=============================================================================

Major Features
-----------------

* Displays diffs side-by-side in the kitty terminal.

* Does syntax highlighting of displayed diffs

* Displays images as well as text diffs, even over SSH

* Does recursive directory diffing


.. figure:: ../screenshots/diff.png
   :alt: Screenshot, showing a sample diff
   :align: center

   Screenshot, showing a sample diff

.. contents::


Installation
---------------

Simply install `kitty <https://github.com/kovidgoyal/kitty>`_.  You also need
to have either the `git <https://git-scm.com/>`_ program or the ``diff`` program
installed. Additionally, for syntax highlighting to work,
`pygments <http://pygments.org/>`_ must be installed (note that pygments is
included in the macOS kitty app).


Usage
--------

In the kitty terminal, run::

    kitty +kitten diff file1 file2

to see the diff between file1 and file2.

Create an alias in your shell's startup file to shorten the command, for example:

.. code-block:: sh

    alias d="kitty +kitten diff"

Now all you need to do to diff two files is::

    d file1 file2

You can also pass directories instead of files to see the recursive diff of the
directory contents.


Keyboard controls
----------------------

=========================   ===========================
Action                      Shortcut
=========================   ===========================
Quit                        ``q, Ctrl+c``
Scroll line up              ``k, up``
Scroll line down            ``j, down``
Scroll page up              ``PgUp``
Scroll page down            ``PgDn``
Scroll to top               ``Home``
Scroll to bottom            ``End``
Scroll to next page         ``Space, PgDn``
Scroll to previous page     ``PgUp``
Scroll to next change       ``n``
Scroll to previous change   ``p``
Increase lines of context   ``+``
Decrease lines of context   ``-``
All lines of context        ``a``
Restore default context     ``=``
=========================   ===========================



Configuring kitty-diff
------------------------

You can configure the colors used, keyboard shortcut, the diff implementation,
the default lines of context, etc.  by creating a diff.conf in your kitty
config folder. The default diff.conf is below.

.. literalinclude:: ../../kittens/diff/diff.conf
    :language: ini



Integrating with git
-----------------------

Add the following to `~/.gitconfig`:

.. code-block:: ini

    [diff]
        tool = kitty
        guitool = kitty.gui
    [difftool]
        prompt = false
        trustExitCode = true
    [difftool "kitty"]
        cmd = kitty +kitten diff $LOCAL $REMOTE
    [difftool "kitty.gui"]
        cmd = kitty kitty +kitten diff $LOCAL $REMOTE

Now to use kitty-diff to view git diffs, you can simply do::

    git difftool --no-symlinks --dir-diff

Once again, creating an alias for this command is useful.


Why does this work only in kitty?
----------------------------------------

The diff kitten makes use of various features that are
:doc:`kitty only </protocol-extensions>`,  such as the
:doc:`kitty graphics protocol </graphics-protocol>`,
the extended keyboard protocol, etc. It also leverages
terminal program infrastructure I created for all of kitty's other kittens to
reduce the amount of code needed (the entire implementation is under 2000 lines
of code).

And fundamentally, it's kitty only because I wrote it for myself, and I am
highly unlikely to use any other terminals :)