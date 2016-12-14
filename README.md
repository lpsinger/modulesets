JHBuild modulesets for LIGO and open-source astronomy software
==============================================================

This is my personal collection of JHBuild (<https://live.gnome.org/Jhbuild>)
modules for automating build and installation of bleeding-edge packages for
LIGO data analysis and open source astronomy.

As a taste, installing all of lalsuite and the attendant Python packages
one-by-one becomes just:

    $ jhbuild build lalsuite

Or, if you'd rather see a progress bar instead of all of the build output,
you can add the `-q` flag:

    $ jhbuild build -q lalsuite

And entering the preconfigured shell with `PATH`, `PKG_CONFIG_PATH`,
`PYTHONPATH`, etc. preconfigured is just:

    $ jhbuild shell


Instructions
------------

To use, first clone this repository into your home directory under
`~/local/src/modulesets` as follows:

    $ mkdir -p ~/local/src
    $ git clone git://github.com/lpsinger/modulesets.git ~/local/src/modulesets

Next, clone and install JHBuild as follows (adapted from
<http://developer.gnome.org/jhbuild/unstable/getting-started.html.en>):

    $ git clone git://git.gnome.org/jhbuild ~/local/src/jhbuild
    $ cd ~/local/src/jhbuild
    $ ./autogen.sh
    $ make
    $ make install

Optionally, add the following line to your login script so that the `jhbuild`
command is in your `PATH`:

    export PATH=$PATH:~/.local/bin

Remember to log out and log back in for the new environment variable to take
effect. Next, symlink the bundled JHBuild configuration file to
`~/.config/jhbuildrc`:

    $ mkdir -p ~/.config && cd ~/.config && ln -s ~/local/src/modulesets/jhbuildrc

Finally, build `lalsuite` with:

    $ jhbuild build lalsuite

To start a shell with your newly built packages in the environment, run:

    $ jhbuild shell


Experimental Support for Intel C Compiler (ICC)
-----------------------------------------------

There is now experimental support for compiling the LIGO/Virgo software stack
(LALSuite) using the Intel C Compiler (`icc`), which is available on LIGO Data
Grid computing clusters. To enable building with `icc`, add the line
`icc = True` to your jhbuild configuration script as follows:

    $ echo 'icc = True' >> ~/.config/my.jhbuildrc


MacPorts
--------

For building LALSuite on [MacPorts](https://www.macports.org), I suggest using
the GCC 5 compiler toolchain (instead of clang) for full OpenMP support. On Mac
OS, make sure that your MacPorts ports are up to date, and then run the
following commands:

    $ sudo port install openmpi-gcc5
    $ sudo port select --set gcc mp-gcc5
    $ sudo port select --set mpi openmpi-gcc5-fortran


Details
-------

- Source code for modules is checked out into `~/src`.

- For packages that support building out-of-srcdir, the build directory is
  in `/usr1/$USER/jhbuild/local`, `/local/$USER/jhbuild/local`,
  `/localscratch/$USER/jhbuild/local` or `/var/tmp/$USER/jhbuild/local`, to
  accommodate scratch storage locations on LSC data analysis clusters.

- Packages are installed into `~/local`.

- You will be reminded whenever you are inside the JHBuild environment shell
  by the colorized prompt beginning with the text `JHBuild:`.

- To set up an additional jhbuild prefix in ~/path/to/local (e.g., if you want
  to do parallel testing of another build configuration or different branches
  of your source repositories), initialize the source tree by running the
  following commands:

      $ mkdir -p ~/path/to/local/src
      $ git clone git://github.com/lpsinger/modulesets.git ~/path/to/local/src/modulesets

  Next, edit the file `~/path/to/local/src/modulesets/jhbuildrc` and change the
  line `prefix = '~/local'` to `prefix = '~/path/to/local'`.

  Finally, pass the options `-f ~/path/to/local/src/modulesets` every time you
  run any JHBuild command. For example:

      $ jhbuild -f ~/path/to/local/src/modulesets build lalsuite
      $ jhbuild -f ~/path/to/local/src/modulesets run lalapps_tconvert
      $ jhbuild -f ~/path/to/local/src/modulesets shell
