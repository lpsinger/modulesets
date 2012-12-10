JHBuild modulesets for LIGO and open-source astronomy software
==============================================================

This is my personal collection of JHBuild <https://live.gnome.org/Jhbuild>
modules for automating build and installation of bleeding-edge packages for
LIGO data analysis and open source astronomy.


To use, first configure git to use your Globus-enabled ssh client by setting the
environment variable ``GIT_SSH=gsissh``. Then, clone this repository into your
home directory under ``~/modulesets`` as follows::

  $ git clone git://github.com/lpsinger/modulesets.git ~/modulesets

Next, clone and install JHBuild as follows (adapted from
<http://developer.gnome.org/jhbuild/unstable/getting-started.html.en>):

  $ mkdir -p src && git clone git://git.gnome.org/jhbuild ~/src/jhbuild
  $ cd ~/src/jhbuild
  $ ./autogen.sh
  $ make
  $ make install

Optionally, add the following line to your login script so that the ``jhbuild``
command is in your ``PATH``::

  export PATH=$PATH:~/.local/bin

Next, symlink the bundled JHBuild configuration file to ``~/.config/jhbuildrc``::

  $ mkdir -p ~/.config && cd ~/.config && ln -s ~/modulesets/jhbuildrc

Finally, build ``lalsuite`` with:

  $ jhbuild build lalsuite

To start a shell with your newly built packages in the environment, run::

  $ jhbuild shell


Details
-------

- Source code for modules is checked out into ``~/src``.

- For packages that support building out-of-srcdir, the build directory is
  in ``/usr1/$USER/build``, ``/local/$USER/build``,
  ``/localscratch/$USER/build`` or ``/var/tmp/$USER/build``, to
  accommodate scratch storage locations on LSC data analysis clusters.

- Packages are installed into ``~/local``.

- You will be reminded whenever you are inside the JHBuild environment shell
  by the colorized prompt beginning with the text ``JHBuild:``.
