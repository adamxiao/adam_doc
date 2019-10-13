# create docker image from iso

refer <https://monotone.github.io/2017/12/17/customised-iso-to-docker-image/>

This is what I do to create a CentOS docker image:

a) A 'Minimal' installation.

b) Remove the following packages.
```bash
yum remove iwl* ql* xorg* ipw* ghostscript* libtheora systemtap-runtime alsa* mysql-libs iw hicolor-icon-theme *firmware* --exclude=kernel-firmware
```

c) Clean the yum cache.
```bash
yum clean all
```

d) Tar up the filesystem excluding the un-wanted locations.
```
tar --numeric-owner --exclude=/proc --exclude=/sys --exclude=/mnt --exclude=/var/cache --exclude=/usr/share/{foomatic,backgrounds,fonts,cups,qt4,groff,kde4,icons,pixmaps,emacs,gnome-background-properties,sounds,gnome,games,desktop-directories}  --exclude=/var/log -zcvf /mnt/centos7-base.tar.gz /
# add new exclude ?
# --exclude=/boot
```
The tar file is around 300MB.

e) Load the image to docker.
```
 # cat centos7-base.tar.gz | docker import - centos7
```
The image size is around 700MB

## Comapre with official centos docker image

1. official images size less than 200MB < 700MB, almost in /usr
2. official rpms total is 150 < 278
3. official not contain /boot
