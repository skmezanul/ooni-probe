ooniprobe: a network interference detection tool
================================================

.. image:: https://travis-ci.org/TheTorProject/ooni-probe.png?branch=master
    :target: https://travis-ci.org/TheTorProject/ooni-probe

.. image:: https://coveralls.io/repos/TheTorProject/ooni-probe/badge.png
    :target: https://coveralls.io/r/TheTorProject/ooni-probe

___________________________________________________________________________

.. image:: https://ooni.torproject.org/images/ooni-header-mascot.svg
    :target: https:://ooni.torproject.org/

OONI, the Open Observatory of Network Interference, is a global observation
network which aims is to collect high quality data using open methodologies,
using Free and Open Source Software (FL/OSS) to share observations and data
about the various types, methods, and amounts of network tampering in the
world.


    "The Net interprets censorship as damage and routes around it."
                - John Gilmore; TIME magazine (6 December 1993)


ooniprobe is the first program that users run to probe their network and to
collect data for the OONI project. Are you interested in testing your network
for signs of surveillance and censorship? Do you want to collect data to share
with others, so that you and others may better understand your network? If so,
please read this document and we hope ooniprobe will help you to gather
network data that will assist you with your endeavors!

Read this before running ooniprobe!
-----------------------------------

Running ooniprobe is a potentially risky activity. This greatly depends on the
jurisdiction in which you are in and which test you are running. It is
technically possible for a person observing your internet connection to be
aware of the fact that you are running ooniprobe. This means that if running
network measurement tests is something considered to be illegal in your country
then you could be spotted.

Furthermore, ooniprobe takes no precautions to protect the install target machine
from forensics analysis.  If the fact that you have installed or used ooni
probe is a liability for you, please be aware of this risk.

OONI in 5 minutes
=================

On debian testing or unstable::

    sudo apt-get install ooniprobe

If you are running debian stable you can get it from backports via::

    sudo sh -c 'echo "deb http://http.debian.net/debian jessie-backports main" >> /etc/apt/sources.list'
    sudo apt-get update && sudo apt-get install ooniprobe

On unix systems::

    sudo pip install ooniprobe

**BUG** Note: Twisted version needs to be >=12.2.0 and <=14.0.0
unistall/downgrage Twisted::

    sudo pip uninstall Twisted
    sudo pip install 'Twisted>=12.2.0,<=14.0.0'

To install it from the current master run::

    sudo pip install https://github.com/TheTorProject/ooni-probe/archive/master.zip

Then run::

    mkdir my_decks
    sudo ooniresources --update-inputs --update-geoip
    oonideckgen -o my_decks/

**BUG** Note:
ooniprobe version 1.2.2 when installed from the debian repository will not
properly create the ooni home folder and if you run into an error in accessing
`~/.ooni/` run::

    ooniprobe -n blocking/http_requests -u http://google.com/

This should generate the home and allow you to run oonideckgen.

The output from the last command will tell you how to run ooniprobe to perform
the measurement.

If you would like to contribute measurements to OONI daily you can also add
this to your crontab::

    @daily ooniprobe $THE_OONI_COMMAND

Run this command to automatically update your crontab:: 

      (crontab -l 2>/dev/null; echo "@daily ooniprobe $THE_OONI_COMMAND") | crontab -

Installation
============

Debian based systems
--------------------

If you are running Debian testing or Debian unstable you can install ooniprobe
simply with::
    
    apt-get install ooniprobe

If you are running Debian stable you can get it from backports via::

    sudo sh -c 'echo "deb http://http.debian.net/debian wheezy-backports main" >> /etc/apt/sources.list'
    sudo apt-get update && sudo apt-get install ooniprobe

If you are running Ubuntu 14.04 LTS, 14.10 or 15.04 you can install it from the PPA
(https://launchpad.net/~irl/+archive/ubuntu/ooni/)::

    sudo add-apt-repository ppa:irl/ooni
    sudo apt-get update && sudo apt-get install ooniprobe

You will be warned that the packages are unauthenticated. This is due to the
PPA not being signed and is normal behaviour. If you would prefer to verify the
integrity of the package, use our private Debian repository below.

Mac OS X
--------

You can install ooniprobe on OSX if you have installed homebrew (http://mxcl.github.io/homebrew) with::

    brew install ooniprobe

Unix systems (with pip)
-----------------------

Make sure you have installed the following depedencies:

  * build-essential
  * python (>=2.7)
  * python-dev
  * pip
  * libgeoip-dev
  * libdumbnet-dev
  * libpcap-dev
  * libssl-dev
  * libffi-dev
  * tor (>=0.2.5.1 to run all the tor related tests)

Then you should be able to install ooniprobe by running::

    pip install ooniprobe

Other platforms (with Vagrant)
------------------------------

1. Install Vagrant (https://www.vagrantup.com/downloads.html) and Install Virtualbox (https://www.virtualbox.org/wiki/Downloads)

2. On OSX:

If you don't have it install homebrew http://mxcl.github.io/homebrew/::

    brew install git

On debian/ubuntu::

    sudo apt-get install git

3. Open a Terminal and run::

    git clone https://git.torproject.org/ooni-probe.git
    cd ooni-probe/
    vagrant up

4. Login to the box with::

    vagrant ssh

ooniprobe will be installed in ``/ooni``.

5. You can run tests with::

    ooniprobe blocking/http_requests -f /ooni/example_inputs/alexa-top-1k.txt

Using ooniprobe
===============

**Net test** is a set of measurements to assess what kind of internet censorship is occurring.

**Decks** are collections of ooniprobe nettests with some associated inputs.

**Collector** is a service used to report the results of measurements.

**Test helper** is a service used by a probe for successfully performing its measurements.

**Bouncer** is a service used to discover the addresses of test helpers and collectors.

Configuring ooniprobe
---------------------

You may edit the configuration for ooniprobe by editing the configuration file
found inside of ``~/.ooni/ooniprobe.conf``.

By default ooniprobe will not include personal identifying information in the
test result, nor create a pcap file. This behavior can be personalized.


Updating resources
------------------

To generate decks you will have to update the input resources of ooniprobe.

This can be done with::

    ooniresources --update-inputs

If you get a permission error, you may have to run the command as root or
change the ooniprobe data directory inside of `ooniprobe.conf`.

On some platforms, for example debian contrib, you will not get all the geoip
related files needed. In that case it is possible to manually download them
with ``ooniresources``::

    ooniresources --update-geoip

Generating decks
----------------

You can generate decks for your country thanks to the oonideckgen command.

If you wish, for example, to generate a deck to be run in the country of Italy,
you can do so (be sure to have updated the input resources first) by running::

    oonideckgen --country-code IT --output ~/

You will now have in your home a folder called `deck-it`, containing the ooni
deck (ends with .deck) and the inputs.
Note: that you should not move the `deck-*` directory once it has been
generated as the paths to the inputs referenced by the test in the deck are
absolute. If you want your deck to live in another directory you must
regenerated it.


Running decks
-------------

You will find all the installed decks inside of ``/usr/share/ooni/decks``.

You may then run a deck by using the command line option ``-i``:

As root::

    ooniprobe -i /usr/share/ooni/decks/mlab.deck


Or as a user::

    ooniprobe -i /usr/share/ooni/decks/mlab_no_root.deck


Or:

As root::

    ooniprobe -i /usr/share/ooni/decks/complete.deck


Or as a user::

    ooniprobe -i /usr/share/ooni/decks/complete_no_root.deck


The above tests will require around 20-30 minutes to complete depending on your network speed.

If you would prefer to run some faster tests you should run:
As root::

    ooniprobe -i /usr/share/ooni/decks/fast.deck


Or as a user::

    ooniprobe -i /usr/share/ooni/decks/fast_no_root.deck


Running net tests
-----------------

You may list all the installed stable net tests with::


    ooniprobe -s


You may then run a nettest by specifying its name for example::


    ooniprobe manipulation/http_header_field_manipulation


It is also possible to specify inputs to tests as URLs::


    ooniprobe blocking/http_requests -f httpo://ihiderha53f36lsd.onion/input/37e60e13536f6afe47a830bfb6b371b5cf65da66d7ad65137344679b24fdccd1


You can find the result of the test in your current working directory.

By default the report result will be collected by the default ooni collector
and the addresses of test helpers will be obtained from the default bouncer.

You may also specify your own collector or bouncer with the options ``-c`` and
``-b``.


Bridges and obfsproxy bridges
=============================

ooniprobe submits reports to oonib report collectors through Tor to a hidden
service endpoint. By default, ooniprobe uses the installed system Tor, but can
also be configured to launch Tor (see the advanced.start_tor option in
ooniprobe.conf), and ooniprobe supports bridges (and obfsproxy bridges, if
obfsproxy is installed). The tor.bridges option in ooniprobe.conf sets the path
to a file that should contain a set of "bridge" lines (of the same format as
used in torrc, and as returned by https://bridges.torproject.org). If obfsproxy
bridges are to be used, the path to the obfsproxy binary must be configured.
See option advanced.obfsproxy_binary, in ooniprobe.conf.

(Optional) Install obfsproxy
----------------------------

Install the latest version of obfsproxy for your platform.

Download Obfsproxy: https://www.torproject.org/projects/obfsproxy.html.en

Setting capabilities on your virtualenv python binary
=====================================================

If your distributation supports capabilities you can avoid needing to run OONI as root::


    setcap cap_net_admin,cap_net_raw+eip /path/to/your/virtualenv's/python


Reporting bugs
==============

You can report bugs and issues you find with ooni-probe on The Tor Projec issue
tracker filing them under the "Ooni" component: https://trac.torproject.org/projects/tor/newticket?component=Ooni.

You can either register an account or use the group account "cypherpunks" with
password "writecode".

Contributing
============

You can download the code for ooniprobe from the following git repository::


    git clone https://git.torproject.org/ooni-probe.git


It is also viewable on the web via: https://gitweb.torproject.org/ooni-probe.git.

You should then submit patches for review as pull requests to this github repository: 

https://github.com/TheTorProject/ooni-probe

Read this article to learn how to create a pull request on github (https://help.github.com/articles/creating-a-pull-request).

If you prefer not to use github (or don't have an account), you may also submit
patches as attachments to tickets.

Be sure to format the patch (given that you are working on a feature branch
that is different from master) with::


    git format-patch master --stdout > my_first_ooniprobe.patch


Setting up development environment
----------------------------------

On Debian based systems a development environment can be setup as follows: (prerequisites include build essentials, python-dev, and tor; for tor see https://www.torproject.org/docs/debian.html.en)::


    sudo apt-get install python-pip python-virtualenv virtualenv virtualenvwrapper
    sudo apt-get install libgeoip-dev libffi-dev libdumbnet-dev libssl-dev libpcap-dev
    git clone https://github.com/TheTorProject/ooni-probe
    cd ooni-probe
    mkvirtualenv ooniprobe  # . ~/.virtualenvs/ooniprobe/bin/activate to access later
    pip install -r requirements.txt
    pip install -r requirements-dev.txt
    python setup.py install
    ooniprobe -s  # if all went well, lists available tests


