
##### Fluidd installation

// excerpt from [README_en](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/blob/main/fluidd/README_en) of [https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/fluidd](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/fluidd)

Instruction

Put the same level directory file ( fluidd ) in the root directory of the U disk

Copy the files ( fluidd.sh and fluidd.tar ) to directory ( /usr/data )
~~~
cp /tmp/udisk/sda1/fluidd/* /usr/data/
~~~

Install fluidd, moonraker and nginx
~~~
/usr/data/fluidd.sh install
~~~

unstall fluidd, moonraker and nginx
~~~
/usr/data/fluidd.sh unstall
~~~

Into fluidd

IP with 4408 port

eg, 192.168.1.1:4408


Warning: Monnraker running for a long time on the K1 series poses a risk of memory overflow

---


WORK IN PROGRESS!
INCOMPLETE and NOT functional.


based on [https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/fluidd](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/fluidd)

Note that the version of Fluidd is a "dirty" version (meaning it's modified by Creality and it's not recommended to update it; otherwise, you lose Creality's tweaks and it will no longer be suited to the CrealityOS environment...)


Moreover, the `fluidd.tar` archive from `Ender-3_V3_KE_Annex` also contains nginx (web server) and moonraker (interface to interact with klipper)
which are also versions modified by Creality to work on Creality OS and with Creality's customized Klippy.

So again, do not update these versions, or you risk crashing the machine and having to reset or recover the firmware.

~~~

~~~

// ??? since the git command is installed by default on CrealityOS, maybe it's simpler to create a GitHub repo for each component to allow relatively easy installation and later updates...
~~~
#cd /usr
#git clone ... /usr/data/...
#/usr/data/.../script_install.sh
#/usr/data/.../script_verif_install.sh
#/usr/data/.../script_uninstall.sh

# cd /usr/data/...; git pull
~~~


// TODO look for a script that downloads and deploys it without having to use the USB drive
~~~
# wget --no-check-certificate https://raw.githubusercontent.com/CrealityOfficial/Ender-3_V3_KE_Annex/main/fluidd/fluidd/fluidd.tar -O /usr/data/fluidd.tar
# wget --no-check-certificate https://raw.githubusercontent.com/CrealityOfficial/Ender-3_V3_KE_Annex/main/fluidd/fluidd/fluidd.sh -O /usr/data/fluidd.sh
# chmod u+x /usr/data/fluidd.sh
# /usr/data/fluidd.sh install

~~~

// TODO consider some kind of cache logic on the USB drive if one is connected.
~~~
ll -R /tmp/udisk/sda1/
/tmp/udisk/sda1/:
total 879532
drwxr-xr-x    7 root     root          4096 Jan  1  1970 ./
drwxr-xr-x    3 root     root            60 Mar  1  2020 ../
drwxr-xr-x    4 root     root          4096 Jan 11 12:23 .Trash-1000/
-rwxr-xr-x    1 root     root       2691227 Oct 28 04:01 1-Boat.gcode*
-rwxr-xr-x    1 root     root       5353033 Dec 19 19:58 2023-12-19_12-57-13_creality_log.7z*
-rwxr-xr-x    1 root     root     858766097 Oct 28 04:08 Ender-3 V3 KE _supplementary files_EN_V1.2.zip*
drwxr-xr-x    2 root     root          4096 Oct 28 05:06 System Volume Information/
drwxr-xr-x    2 root     root          4096 Feb  1 00:44 _done/
drwxr-xr-x    2 root     root          4096 Jan 11 12:23 fluidd/
-rwxr-xr-x    1 root     root      33797120 Feb  1 00:45 fluidd.tar*
drwxr-xr-x    2 root     root          4096 Jan 11 12:23 mainsail/



/tmp/udisk/sda1/fluidd:
total 82732
drwxr-xr-x    2 root     root          4096 Jan 11 12:23 ./
drwxr-xr-x    7 root     root          4096 Jan  1  1970 ../
-rwxr-xr-x    1 root     root           682 Jan  9 17:27 fluidd.sh*
-rwxr-xr-x    1 root     root      84705280 Jan  9 17:27 fluidd.tar*

/tmp/udisk/sda1/mainsail:
total 57472
drwxr-xr-x    2 root     root          4096 Jan 11 12:23 ./
drwxr-xr-x    7 root     root          4096 Jan  1  1970 ../
-rwxr-xr-x    1 root     root           686 Jan  9 17:27 mainsail.sh*
-rwxr-xr-x    1 root     root      58839040 Jan  9 17:27 mainsail.tar*

md5sum /tmp/udisk/sda1/fluidd/*
4b579565afd2cee990d9eb997c87e5ac  /tmp/udisk/sda1/fluidd/fluidd.sh
2036ec028fc96acfd01359e30e21c64c  /tmp/udisk/sda1/fluidd/fluidd.tar

md5sum /tmp/udisk/sda1/mainsail/*
8dc3a1ab0f0be82aa00b573233e16797  /tmp/udisk/sda1/mainsail/mainsail.sh
105e1215ba1c5dc6fa824fe33878faa2  /tmp/udisk/sda1/mainsail/mainsail.tar


cp /tmp/udisk/sda1/fluidd/* /usr/data/
~~~


// TODO look for a verification script
~~~
#
if [ -d /usr/data/nginx ]; then echo "OK"; else echo "KO"; fi

      #/usr/data/nginx /usr/data/moonraker /usr/data/fluidd

mypath="/usr/data/nginx"; if [ -d $mypath ]; then echo "OK $mypath"; else echo "KO $mypath"; fi
mypath="/usr/data/nginxsss"; if [ -d $mypath ]; then echo "DIR OK $mypath"; else echo "DIR KO $mypath"; fi

mypath="/etc/init.d/S50nginx"; if [ -f $mypath ]; then echo "OK $mypath"; else echo "KO $mypath"; fi

~~~


https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/blob/main/fluidd/README_en
~~~
Instruction

Put the same level directory file ( fluidd ) in the root directory of the U disk

Copy the files ( fluidd.sh and fluidd.tar ) to directory ( /usr/data )
cp /tmp/udisk/sda1/fluidd/* /usr/data/

Install fluidd, moonraker and nginx
/usr/data/fluidd.sh install

unstall fluidd, moonraker and nginx
/usr/data/fluidd.sh unstall

Into fluidd
IP with 4408 port
eg, 192.168.1.1:4408

Warning: Monnraker running for a long time on the K1 series poses a risk of memory overflow
~~~

https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/raw/main/fluidd/fluidd/fluidd.tar
(
If we have already installed the mainsail from Ender-3_V3_KE_Annex, which also includes/install nginx and moonraker,
we can simply extract the fluidd folder from the fluidd.tar archive because it contains the same nginx and fluidd...)
~~~
tar -C /usr/data -xvf /usr/data/fluidd.tar "fluidd"
~~~
and not use the install script because the moonraker and nginx services are already installed and running.
)
https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/blob/main/fluidd/fluidd/fluidd.sh
~~~bash
#!/bin/sh

if [ "x$1" = "xinstall" ]; then
    cd /usr/data
    tar -xvf fluidd.tar 

    [ ! -e /etc/init.d/S50nginx ] && mv nginx/S50nginx /etc/init.d/
    [ ! -e /etc/init.d/S56moonraker_service ] && mv moonraker/S56moonraker_service /etc/init.d/

    /etc/init.d/S50nginx start
    /etc/init.d/S56moonraker_service start
elif [ "x$1" = "xunstall" ]; then
    cd /overlay/upper
    /etc/init.d/S50nginx stop
    /etc/init.d/S56moonraker_service stop
    rm -rf /etc/init.d/S50nginx /etc/init.d/S56moonraker_service 
    rm -rf /usr/data/printer_data/config/moonraker.conf /usr/data/printer_data/config/.moonraker.conf.bkp /usr/data/nginx /usr/data/moonraker /usr/data/fluidd
fi
~~~


