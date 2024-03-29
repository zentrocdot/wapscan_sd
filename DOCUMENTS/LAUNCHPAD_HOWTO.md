# Launchpad How-To

<p align="justify">Building a package for <code>Launchpad</code> is a little bit different form building a DEB package for personal use. There are a few more rules to follow.</p>

<p align="justify">I am using the structure which I presented in the other  <a href="https://github.com/zentrocdot/wapscan_package/blob/main/HOW-TO.md">How-To<a>

# Preparation 

<p align="justify">Delete all package related files in the parent folder.</p>

    ~/USR_DEB

<p align="justify">First re-creating the tree structure from within the package directory e.g.</p>

    ~/USR_DEB/wapscan-0.0.0.2-all

# Prepare Files

<p align="justify">We have to make some modifications in debian/control and debian/changelog:</p>  

    wapscan (0.0.0.2-all) UNRELEASED; urgency=medium    # Results in a rejected package   

    wapscan (0.0.0.2-all) jammy; urgency=medium

 <p align="justify">A package which has UNRELEASED in the changlog file will be rejected.</p>   

 <p align="justify">The file control has also to be changed.</p>  

    Section: unknown    # Results in a rejected package   

    Section: utils

Allowed are:

admin, cli-mono, comm, database, debug, devel, doc, editors, education, electronics, embedded, fonts, games, gnome, gnu-r, gnustep, graphics, hamradio, haskell, httpd, interpreters, introspection, java, javascript, kde, kernel, libdevel, libs, lisp, localization, mail, math, metapackages, misc, net, news, ocaml, oldlibs, otherosfs, perl, php, python, ruby, rust, science, shells, sound, tasks, tex, text, utils, vcs, video, web, x11, xfce, zope

# Create Package for Distribution

<p align="justify">using the command dh_make:</p>  

    dh_make -y --indep --createorig

<p align="justify">Thean use debuild.</p>  

    debuild -S -sa    # New from scratch

    debuild -S -sd    # Difference build

<p align="justify">Ignore the signing error.</p>  

<p align="justify">Change to the parent directory.</p> 

    debsign -k <key-ID> wapscan_0.0.0.1-all-1_source.changes

<p align="justify">Afterwards the package can be uploaded.</p> 

    dput ppa:zentrocdot/wapscan-cli-ppa wapscan_0.0.0.1-all-1_source.changes

<p align="justify">If one makes changes, fore upload.</p> 

    dput --force ppa:zentrocdot/wapscan-cli-ppa wapscan_0.0.0.1-all-1_source.changes

# Passwords & Encryption Keys Application

<p align="justify">The passwords & encryption keys application name is unusually seahorse.</p> 

    sudo apt-get install seahorse

# Getting the KEY ID

    gpg2 --list-keys --keyid-format LONG

# File Content

File <code>changelog</code>

    wapscan (0.0.0.1-all) jammy; urgency=medium

      * Initial release. 

     -- Dr. Peter Netz <zentrocdot@protonmail.com>  Mon, 04 Mar 2024 09:22:23 +0100

File <code>control</code>

    Source: wapscan
    Section: utils
    Priority: optional
    Maintainer: Dr. Peter Netz <zentrocdot@protonmail.com>
    Build-Depends: debhelper-compat (= 13)
    Standards-Version: 4.6.0
    Rules-Requires-Root: no

    Package: wapscan
    Architecture: all
    Depends: ${misc:Depends}, wireless-tools, iw, sed, gawk
    Description: Wireless Access Point Scanner
     Scanning for nearby wireless access points

File <code>control</code>

    Format: https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
    Upstream-Name: wapscan
    Upstream-Contact: Dr. Peter Netz, <zentrocdot@neutronmail.org>
    Source: https://github.com/zentrocdot/wapscan_package

    Files: debian/*
    Copyright: 2017-2024, Dr. Peter Netz, <zentrocdot@neutronmailmail.org>
    License: MIT

    Copyright: 2024 Dr. Peter Netz <hades@unknown>
    License: GPL-2+
    This package is free software; you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation; either version 2 of the License, or
    (at your option) any later version.
 
    This package is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.
 
    You should have received a copy of the GNU General Public License
    along with this program. If not, see <https://www.gnu.org/licenses/>

    On Debian systems, the complete text of the GNU General
    Public License version 2 can be found in "/usr/share/common-licenses/GPL-2".

In file format this has to be changed:

    3.0 (quilt)

to

   3.0 (native)

The file wapscan.install looks like

    ./usr/share/wapscan/oui/* /usr/share/wapscan
    ./usr/bin/* /usr/bin

# References

[1]   askubuntu.com/questions/1087569/deploying-own-debian-package-to-launchpad

[2]   askubuntu.com/questions/140803/steps-for-creating-a-slightly-modified-package-and-uploading-it-in-a-ppa

[3]    askubuntu.com/questions/1012403/how-to-upload-build-a-custom-kernel-on-launchpad

[4]    help.launchpad.net/YourAccount/CreatingAnSSHKeyPair

[5]    help.launchpad.net/Packaging/PPA/BuildingASourcePackage

[6]    help.launchpad.net/Packaging/PPA/Uploading

[7]    help.launchpad.net/ReadingOpenPgpMail

[8]    help.launchpad.net/Packaging/UploadErrors

[9]    help.ubuntu.com/community/GnuPrivacyGuardHowto

[10]   launchpad.net/~zentrocdot/+archive/ubuntu/wapscan-cli-ppa

[11]    stackoverflow.com/questions/43699845/error-uploading-files-for-distribution-unreleased-to-ppa-not-allowed

[12]    stackoverflow.com/questions/50089033/how-do-i-get-the-changes-file-for-a-debian-package

[13]    stackoverflow.com/questions/46879602/get-fingerprints-of-openpgp-keys

[14]    wiki.ubuntuusers.de/GnuPG/

[15]    www&#8203;.debian.org/doc/debian-policy/ch-controlfields.html

[16]    askubuntu.com/questions/226495/how-to-solve-dpkg-source-source-problem-when-building-a-package

[17]    www&#8203;.debian.org/doc/debian-policy/ch-archive.html

[18]    askubuntu.com/questions/737884/debuild-secret-key-not-available-someone-elses-key

[19]   www&#8203;.debian.org/doc/manuals/maint-guide/build.en.html

[20]    debian-handbook.info/browse/stable/sect.package-meta-information.html

[30]    unix.stackexchange.com/questions/148503/changelog-of-deb-package

[31]    askubuntu.com/questions/579323/dch-non-interactive-mode

[32]    askubuntu.com/questions/754809/whats-the-meaning-of-uc-us-options-in-debuild-uc-us

[33]    www&#8203;.debian.org/doc/packaging-manuals/copyright-format/1.0/; Machine-readable debian/copyright file

[34]    www&#8203;.debian.org/doc/debian-policy/ch-relationships.html#s-sourcebinarydeps

[35]    www&#8203;.debian.org/doc/packaging-manuals/copyright-format/1.0/

[36]    www&#8203;.baeldung.com/linux/create-debian-package

[37]    stackoverflow.com/questions/21019144/how-to-create-manual-entry-for-deb-package

[38]    wiki.debian.org/Packaging

[39]    wiki.debian.org/HowToPackageForDebian

[40]    wiki.debian.org/Packaging/Intro

[41]    wiki.ubuntuusers.de/Grundlagen_der_Paketerstellung/

[42]   www&#8203;.debian.org/doc/manuals/debmake-doc/ch04.en.html
