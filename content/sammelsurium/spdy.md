---
title: spdy
date: 2012-02-01T17:44:24.000000
tags: 
- Software
- Apache
---


http://code.google.com/p/mod-spdy/issues/detail?id=19

http://code.google.com/p/mod-spdy/wiki/GettingStarted

http://code.google.com/p/mod-spdy/wiki/HowToBuild

http://japhr.blogspot.com/2011/04/dont-bother-with-modspdy.html

## Quick Build for new version #

~~~
cd ~/Code/spdy/mod_spdy
../depot_tools/gclient update --force
cd src
make
sudo cp out/Debug/libmod_spdy.so /usr/lib/apache2/modules/libmod_spdy.so
/etc/init.d/apache2 restart
~~~

## Load Apache2 File #

    LoadModule spdy_module /usr/lib/apache2/modules/libmod_spdy.so
    echo "SpdyEnabled on" | sudo tee /etc/apache2/mods-available/spdy.conf