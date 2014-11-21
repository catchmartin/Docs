* install libraries
  * git autoconf gperf bison flex texinfo libtool libncurses-devel wget gawk gcc-core gcc-g++ mingw-gcc-core mingw-gcc-g++ gccmakedep make automake libexpat-devel python patch libintl-devel openssl-devel gnutls-devel
  * `wget https://bootstrap.pypa.io/ez_setup.py -O - | python; easy_install -U pyserial`

```
cd /opt/Espressif
git clone -b lx106 git://github.com/jcmvbkbc/crosstool-NG.git 
cd crosstool-NG
```

* patch files as follows
```
--- crosstool-ng-1.15.0_old/kconfig/Makefile    2012-04-30 16:54:20.000000000 -0400
+++ crosstool-ng-1.15.0/kconfig/Makefile        2012-05-08 11:10:24.950066100 -0400
@@ -35,20 +35,21 @@
 conf_OBJ = $(patsubst %.c,%.o,$(conf_SRC))
 conf_DEP = $(patsubst %.o,%.dep,$(conf_OBJ))
 $(conf_OBJ) $(conf_DEP): CFLAGS += $(INTL_CFLAGS)
+conf: LDFLAGS += -lintl

 # What's needed to build 'mconf'
 mconf_SRC = mconf.c
 mconf_OBJ = $(patsubst %.c,%.o,$(mconf_SRC))
 mconf_DEP = $(patsubst %.c,%.dep,$(mconf_SRC))
 $(mconf_OBJ) $(mconf_DEP): CFLAGS += $(NCURSES_CFLAGS) $(INTL_CFLAGS)
-mconf: LDFLAGS += $(NCURSES_LDFLAGS)
+mconf: LDFLAGS += -lintl $(NCURSES_LDFLAGS)

 # What's needed to build 'nconf'
 nconf_SRC = nconf.c nconf.gui.c
 nconf_OBJ = $(patsubst %.c,%.o,$(nconf_SRC))
 nconf_DEP = $(patsubst %.c,%.dep,$(nconf_SRC))
-$(nconf_OBJ) $(nconf_DEP): CFLAGS += $(INTL_CFLAGS)
-nconf: LDFLAGS += -lmenu -lpanel -lncurses
+$(nconf_OBJ) $(nconf_DEP): CFLAGS += -I/usr/include/ncurses/ $(INTL_CFLAGS)
+nconf: LDFLAGS += -lintl -lmenu -lpanel -lncurses

 # Under Cygwin, we need to auto-import some libs (which ones, exactly?)
 # for mconf and nconf to lin properly.


--- crosstool-ng-1.15.0_old/kconfig/nconf.c     2012-04-30 16:54:20.000000000 -0400
+++ crosstool-ng-1.15.0/kconfig/nconf.c 2012-05-07 16:47:39.618358900 -0400
@@ -1518,7 +1518,7 @@
        }

        notimeout(stdscr, FALSE);
-       ESCDELAY = 1;
+       set_escdelay(1);

        /* set btns menu */
        curses_menu = new_menu(curses_menu_items);
```

```
./bootstrap && ./configure --prefix=`pwd` && make && make install
./ct-ng xtensa-lx106-elf
```

* Edit registry to make cygwin case sensitive
  * If you really want case-sensitivity in Cygwin, you can switch it on by setting the registry value
  * HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel\obcaseinsensitive
  * to 0 and reboot the machine.

`./ct-ng build`

Took about 100 mins

`PATH=$PWD/builds/xtensa-lx106-elf/bin:$PATH`


```
cd /opt/Espressif
wget -O esp_iot_sdk_v0.9.2_14_10_24.zip http://bbs.espressif.com/download/file.php?id=9
unzip esp_iot_sdk_v0.9.2_14_10_24.zip
mv esp_iot_sdk_v0.9.2 ESP8266_SDK
cd ESP8266_SDK
sed -i -e 's/xt-ar/xtensa-lx106-elf-ar/' -e 's/xt-xcc/xtensa-lx106-elf-gcc/' -e 's/xt-objcopy/xtensa-lx106-elf-objcopy/' Makefile
mv examples/IoT_Demo .
wget -O lib/libc.a https://github.com/esp8266/esp8266-wiki/raw/master/libs/libc.a
wget -O lib/libhal.a https://github.com/esp8266/esp8266-wiki/raw/master/libs/libhal.a
wget -O include.tgz https://github.com/esp8266/esp8266-wiki/raw/master/include.tgz
tar -xvzf include.tgz
```

* `cd ..`
* Get the esptool-0.0.2.zip file from http://www.esp8266.com/viewtopic.php?f=9&t=142&start=20#p772
* unzip esptool directory inside archive to /opt/Espressif/ESP8266_SDK/
```
cd /opt/Espressif
git clone https://github.com/themadinventor/esptool esptool-py
chmod -R +w crosstool-NG/builds/xtensa-lx106-elf
ln -s $PWD/esptool-py/esptool.py crosstool-NG/builds/xtensa-lx106-elf/bin/
```
