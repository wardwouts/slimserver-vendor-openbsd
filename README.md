slimserver-vendor
=================

Fork of the slimserver-vendor repo patched to work with openbsd.

The following installation instructions are largely based on Roland0's work in `http://forums.slimdevices.com/archive/index.php/t-99648.html`

Download `http://downloads.slimdevices.com/LogitechMediaServer_v7.8.0/logitechmediaserver-7.8.0-noCPAN.tgz` and extract in `/opt`:

```
$ sudo mkdir -p /opt
$ wget http://downloads.slimdevices.com/LogitechMediaServer_v7.8.0/logitechmediaserver-7.8.0-noCPAN.tgz
$ sudo tar xzvf logitechmediaserver-7.8.0-noCPAN.tgz -C /opt
```

Fix the permissions:
```
$ sudo chown -R root:root /opt/logitechmediaserver-7.7.2-33893-noCPAN/
```
