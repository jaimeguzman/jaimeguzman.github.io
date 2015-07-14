
The kernel is universally detected with 'uname':

```
$ uname -or
2.6.18-128.el5 GNU/Linux

```

There really isn't a cross-distribution way to determine what distribution and version you're on. There are attempts to make this consistent, but it ultimately varies, unfortunately. LSB tools provide this information, but ironically aren't installed by default everywhere. Example on an Ubuntu 9.04 system with the lsb-release package installed:

```
$ lsb_release -irc
Distributor ID: Ubuntu
Release:        9.04
Codename:       jaunty

```

Otherwise, the closest widely-available method is checking "/etc/something-release" files. These are common on most of the common platforms, or on their derivatives (ie Red Hat and CentOS).

Here's some examples.

For example, Ubuntu has /etc/lsb-release:

```
cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=9.04
DISTRIB_CODENAME=jaunty
DISTRIB_DESCRIPTION="Ubuntu 9.04"

```

But Debian has /etc/debian_version:

```
cat /etc/debian_version
5.0.2

```

Fedora, Red Hat and CentOS have:

```
Fedora: cat /etc/fedora-release
Fedora release 10 (Cambridge)
Red Hat/CentOS: cat /etc/redhat-release
CentOS release 5.3 (Final)

```

Gentoo:

```
cat /etc/gentoo-release
Gentoo Base System release 1.12.11.1

```

I don't have a SUSE system available at the moment, but I believe it is /etc/SuSE-release.

Slackware has /etc/slackware-release.

Mandriva has /etc/mandriva-release.

For most of the popular distributions then,

```
cat /etc/*{release,version}

```

Will most often work. Stripped down and barebones "server" installations might not have the 'release' package for the distribution installed.

Additionally, two 3rd party programs you can use to automatically get this information are Ohai and Facter.

Note that many distributions have this kind of information in /etc/issue or /etc/motd, but some security policies and [best practices](http://cisecurity.org/) indicate that these files should contain access notification banners.

