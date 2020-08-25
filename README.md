Building Software Defined Radio (SDR) systems based on meta-sdr
=============================================
This repository provides git submodules to setup the OpenEmbedded build system
with meta-sdr and possibly one or more BSP's (based on the branch you select).

OpenEmbedded allows the creation of custom linux distributions for embedded
systems. It is a collection of git repositories known as *layers* each of
which provides *recipes* to build software packages as well as configuration
information.

Information about the branch names is available at
https://wiki.yoctoproject.org/wiki/Releases. Helpful articles about working
with GNUradio and Openembedded are at: http://www.opensdr.com/categories/.

Getting Started
---------------

1. Clone the git repository:

```
    $ git clone https://github.com/balister/sdr-build-e320.git
```

2. Check out the appropriate branch:

```
    $ cd sdr-build-e320
    $ git checkout -b zeus-gnuradio origin/zeus-gnuradio
```

3. Update the submodules:

```
    $ git submodule update --init
```

4. Initialize the build system:

```
    $ TEMPLATECONF=`pwd`/meta-sdr/conf/conf-e3xx source ./openembedded-core/oe-init-build-env ./build ./bitbake
```

5. Select the MACHINE to build for:

```
    $ export MACHINE="ni-e31x-sg3"   (default from local.conf)
```

6. Build an image:

```
    $ bitbake gnuradio-dev-image
```

7. Build another image:

```
    $ bitbake gnuradio-demo-image
```

8. Build and sdk:

```
    $ bitbake -c populate_sdk gnuradio-dev-image
```

Day to Day
----------

1. Setup environment after first setup.

```
    $ cd <top of build>
    $ source ./openembedded-core/oe-init-build-env ./build ./bitbake
```

2. Check for updates.

```
    $ cd <top of build>
    $ git pull
    $ git submodule update --init
```

Copying the image to an SD card
-------------------------------

1. Install bmaptool from your distro's package manager. (Fedora and Ubuntu)

2. Insert the card into the writer, if partitions automount, un mount them.

3. Figure out what device the card is. Something like /dev/sdh.

4. Run

```
    $ sudo bmaptool copy tmp-glibc/deploy/images/ni-e31x-sg3/gnuradio-dev-image-ni-e31x-sg3.wic.gz /dev/sdX
```

5. Replace X with the correct letter.

Notes
-----
    * Uses meta-ettus from my repo so I can stage fixes
    * Includes the mender repo, but isn't using them
    * I really need to refactor this so I can build e320 and e3xx machines
      without playing with bblayers.
