slimserver-vendor
=================

Fork of the slimserver-vendor repo patched to work with openbsd.

## Installing LMS

The following installation instructions are largely based on Roland0's work in `http://forums.slimdevices.com/archive/index.php/t-99648.html`. Thanks Roland0!

### Perl

Download the perl sources from `http://www.cpan.org/src/README.html`. Untar and run the configuration:

```
wget http://www.cpan.org/src/5.0/perl-5.14.4.tar.gz
tar xzvf perl-5.14.4.tar.gz
cd perl-5.14.4
./Configure -des -Dprefix=/opt/perl-5.14.4-LMS -Dusethreads
```

Compile and install perl:

```
make
make test
sudo make install-strip
```

As far as I can see there is no real reason LMS can't work with the base perl version, but this does keep all the many many add-ons nicely separate from the nice and clean OpenBSD install.

### LMS

Download `http://downloads.slimdevices.com/LogitechMediaServer_v7.8.0/logitechmediaserver-7.8.0-noCPAN.tgz` and extract in `/opt`:

```
sudo mkdir -p /opt
wget http://downloads.slimdevices.com/LogitechMediaServer_v7.8.0/logitechmediaserver-7.8.0-noCPAN.tgz
sudo tar xzvf logitechmediaserver-7.8.0-noCPAN.tgz -C /opt
```

Fix the permissions:
```
sudo chown -R root:root /opt/logitechmediaserver-7.7.2-33893-noCPAN/
```

### Perl modules
Clone this repo and run buildme.sh:

```
git clone https://github.com/wardwouts/slimserver-vendor-openbsd.git slimserver-vendor-openbsd.git
cd slimserver-vendor-openbsd.git/CPAN
./buildme.sh
```

If the build was succesful copy the modules (fill in the right architecture name `OpenBSD.powerpc-openbsd-thread-multi` in my case):
```
sudo mkdir -p /opt/logitechmediaserver-7.8.0-noCPAN/CPAN/arch/5.14/
cd build/arch/5.14/
sudo cp -R <architecture e.g. x86_64-linux-thread-multi>/ /opt/logitechmediaserver-7.8.0-noCPAN/CPAN/arch/5.14
```

### Running the server

Create a group for the squeezecenter daemon if you haven't already (make sure you pick a unique groupid, I picked 1001 for my system):

```
sudo sh -c "echo _squeezecenter:*:1001: >> /etc/group"
```

And create a user for the daemon:

```
sudo vipw
```

and add the following line (again changing the ids to be unique)

```
_squeezecenter:*************:1001:1001:daemon:0:0:squeezeboxserver:/nonexistent:/sbin/nologin
```

Copy the `squeezecenter` script from your git clone to `/etc/rc.d`:

```
sudo cp slimserver-vendor-openbsd.git/squeezecenter /etc/rc.d
sudo chmod 0555 /etc/rc.d/squeezecenter
```

Add `squeezecenter` to the `pkg_scripts` line in `/etc/rc.conf.local` (create it if you don't have one yet):

```
pkg_scripts="squeezecenter"
```

Create the needed directories:

```
sudo mkdir -p /var/db/squeezecenter /var/log/squeezecenter /var/cache/squeezecenter
sudo chown _squeezecenter:_squeezecenter /var/db/squeezecenter
sudo chown _squeezecenter:_squeezecenter /var/log/squeezecenter
sudo chown _squeezecenter:_squeezecenter /var/cache/squeezecenter
```

Finally start the server:
```
cd /etc/rc.d
sudo ./squeezecenter start
```
