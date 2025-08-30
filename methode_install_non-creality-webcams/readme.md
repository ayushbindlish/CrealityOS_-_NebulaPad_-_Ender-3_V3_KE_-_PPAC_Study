
For a version of this tutorial with screenshots (but with fewer command results)
[https://www.lesimprimantes3d.fr/tutoriel-installer-webcam-logitech-creality-ender-3-v3-ke-20240209/](https://www.lesimprimantes3d.fr/tutoriel-installer-webcam-logitech-creality-ender-3-v3-ke-20240209/)


2024-03-11 **Since the publication of this tutorial, Guilouz has made some changes. I have updated here the links and commands that have changed**


Prerequisites
- firmware v1.1.0.12 with root mode enabled
- `opkg` command provided by Entware, installable via Guilouz's "[Helper Script Installation](https://guilouz.github.io/Creality-K1-Series/helper-script/helper-script-installation/)" (see  [https://guilouz.github.io/Creality-K1-Series/helper-script/entware/](https://guilouz.github.io/Creality-K1-Series/helper-script/entware/) )
~~~
git clone https://github.com/Guilouz/Creality-Helper-Script.git /usr/data/helper-script
sh /usr/data/helper-script/helper.sh
~~~
After installing Entware, you must
either run the command
~~~
export PATH="/opt/bin:/opt/sbin:$PATH"
~~~
or disconnect and reopen an ssh connection (so that the PATH environment variable is up to date because the installation placed a /etc/profile.d/entware.sh that is automatically executed when a session starts.)

<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
root@F005-4A88 /root [#] opkg
-sh: opkg: not found
root@F005-4A88 /root [#] 

root@F005-4A88 /root [#] cat /etc/profile.d/entware.sh 
export PATH="/opt/bin:/opt/sbin:$PATH"
root@F005-4A88 /root [#] 

root@F005-4A88 /root [#] export PATH="/opt/bin:/opt/sbin:$PATH"
root@F005-4A88 /root [#] 

root@F005-4A88 /root [#] which opkg
/opt/bin/opkg
root@F005-4A88 /root [#] 

root@F005-4A88 /root [#] opkg list-installed
entware-opt - 227000-3
entware-release - 1.0-2
entware-upgrade - 1.0-1
findutils - 4.9.0-1a
grep - 3.8-2
libc - 2.27-11
libgcc - 8.4.0-11
libpcre2 - 10.42-1
libpthread - 2.27-11
librt - 2.27-11
libssp - 8.4.0-11
libstdcpp - 8.4.0-11
locales - 2.27-9
opkg - 2022-02-24-d038e5b6-2
terminfo - 6.4-2
zoneinfo-asia - 2023c-2
zoneinfo-core - 2023c-2
zoneinfo-europe - 2023c-2
root@F005-4A88 /root [#] 
</pre>
</details>

---

Use `opkg` (provided by Entware) to install the following packages
~~~
opkg install mjpg-streamer
~~~
<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
root@F005-4A88 /root [#] opkg install mjpg-streamer
Installing mjpg-streamer (1.0.0-6) to root...
Downloading http://bin.entware.net/mipselsf-k3.4/mjpg-streamer_1.0.0-6_mipsel-3.4.ipk
Installing libjpeg-turbo (2.1.4-2) to root...
Downloading http://bin.entware.net/mipselsf-k3.4/libjpeg-turbo_2.1.4-2_mipsel-3.4.ipk
Installing libiconv-full (1.17-1) to root...
Downloading http://bin.entware.net/mipselsf-k3.4/libiconv-full_1.17-1_mipsel-3.4.ipk
Installing libv4l (1.22.1-1) to root...
Downloading http://bin.entware.net/mipselsf-k3.4/libv4l_1.22.1-1_mipsel-3.4.ipk
Configuring libiconv-full.
Configuring libjpeg-turbo.
Configuring libv4l.
Configuring mjpg-streamer.
root@F005-4A88 /root [#] 
</pre>
</details>

~~~
opkg install mjpg-streamer-input-uvc
~~~
<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
// TODO
</pre>
</details>

~~~
opkg install mjpg-streamer-output-http
~~~
<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
root@F005-4A88 /root [#] opkg install mjpg-streamer-output-http
Installing mjpg-streamer-output-http (1.0.0-6) to root...
Downloading http://bin.entware.net/mipselsf-k3.4/mjpg-streamer-output-http_1.0.0-6_mipsel-3.4.ipk
Configuring mjpg-streamer-output-http.
root@F005-4A88 /root [#] 
</pre>
</details>

Check that your webcam is properly connected (Here, I have a Logitech C170)
~~~
lsusb
~~~
<pre>
root@F005-4A88 /root [#] lsusb
Bus 001 Device 003: ID 046d:082b Logitech, Inc. Webcam C170
Bus 001 Device 002: ID 05e3:0610 Genesys Logic, Inc. 4-port hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
root@F005-4A88 /root [#] 
</pre>


Check that v4l (Video for Linux) then finds a video device
~~~
v4l2-ctl --list-devices
~~~
<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
root@F005-4A88 /root [#] v4l2-ctl --list-devices
jz-rot ():
	/dev/video0

Webcam C170 (1.2):
	/dev/media0

Dummy video device (0x0000) (platform:v4l2loopback-000):
	/dev/video3

Webcam C170 (usb-13500000.otg_new-1.2):
	/dev/video4

vpu-felix (vpu-felix):
	/dev/video2

vpu-helix (vpu-helix):
	/dev/video1

root@F005-4A88 /root [#] 
</pre>
</details>

To get only the video device(s) we need here
~~~
v4l2-ctl --list-devices|grep -A1 usb|sed 's/^[[:space:]]*//g'|grep '^/dev'
~~~
In my case this gives
<pre>
/dev/video4
</pre>


To know the resolutions and formats available for this video device here `/dev/video4` (this may change for you, so adapt the following command as needed)
~~~
v4l2-ctl -d /dev/video4 --list-formats-ext
~~~
<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
root@F005-4A88 /root [#] v4l2-ctl -d /dev/video4 --list-formats-ext
ioctl: VIDIOC_ENUM_FMT
	Type: Video Capture

	[0]: 'YUYV' (YUYV 4:2:2)
		Size: Discrete 640x480
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 352x288
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 320x240
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 176x144
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 160x120
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 544x288
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 432x240
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 320x176
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 640x360
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
	[1]: 'MJPG' (Motion-JPEG, compressed)
		Size: Discrete 640x480
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 352x288
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 320x240
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 176x144
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 160x120
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 544x288
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 432x240
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 320x176
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 640x360
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 800x480
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
		Size: Discrete 1024x768
			Interval: Discrete 0.033s (30.000 fps)
			Interval: Discrete 0.067s (15.000 fps)
root@F005-4A88 /root [#] 
</pre>
</details>

---

Download the script https://openk1.org/static/k1/scripts/multi-non-creality-webcams.sh
(but the command didn't work for me ... ? http vs https? So I downloaded it on my PC and then transferred it via `sftp` previously installed with `opkg` ...)
~~~
wget --no-check-certificate -O- 'https://openk1.org/static/k1/scripts/multi-non-creality-webcams.sh' > /etc/init.d/S50non_creality_webcam
~~~

or copy and paste the following block to create a `/etc/init.d/S50non_creality_webcam` file adapted for my Logitech C170 (resolution changed with `-r 640x360`)
~~~ bash
cat > /etc/init.d/S50non_creality_webcam<<'===EOF==='
#!/bin/sh
# S50non_creality_webcam - by destinal
# Start up mjpg-streamer on port 8080 for non-creality cameras (where cam_app doesn't autostart)

# PPAC notes:
## Original script source:
# https://openk1.org/static/k1/scripts/multi-non-creality-webcams.sh
## Prerequisites
# - the result of an "lsusb" must show an additional line when the webcam is plugged into one of the NebulaPad's USB ports.
# - install Entware via Guilouz's script to have the "opkg" command see https://github.com/Guilouz/Creality-K1-and-K1-Max/wiki/Entware
# - opkg install mjpg-streamer
# - opkg install mjpg-streamer-input-uvc
# - opkg install mjpg-streamer-output-http
# - Adjust the resolution via the '-r' argument of mjpg_streamer (see "v4l2-ctl --list-devices|grep -A1 usb|sed 's/^[[:space:]]*//g'|grep '^/dev'" and "v4l2-ctl -d /dev/video4 --list-formats-ext")
# End of PPAC notes.

case "$1" in
  start)
    # V4L_DEV=$(v4l2-ctl --list-devices|grep -A1 usb|tail -1|sed 's/^[[:space:]]*//g')
    V4L_DEVS=$(v4l2-ctl --list-devices|grep -A1 usb|sed 's/^[[:space:]]*//g'|grep '^/dev')
    CREALITY_CAMS=$(v4l2-ctl --list-devices|grep CREALITY|wc -l)
    if [ "x$V4L_DEVS" = "x" -o $CREALITY_CAMS -gt 0 ]; then
      echo "No third party webcams found or we have a creality camera. Bailing!"
      exit 1
    fi

    # Otherwise we're all set up and have a device ID
    PORT=8080
    for V4L_DEV in $V4L_DEVS; do
      /opt/bin/mjpg_streamer -b -i "/opt/lib/mjpg-streamer/input_uvc.so -d $V4L_DEV -r 640x360 -f 15" -o "/opt/lib/mjpg-streamer/output_http.so -p $PORT"
      PORT=`expr $PORT + 1` 
    done
    ;;
  stop)
    killall mjpg_streamer
    ;;
  restart|reload)
    "$0" stop
    "$0" start
    ;;
  *)
    echo "Usage:"
    echo "    $0 {start|stop|restart}"
    exit 1
esac

exit $?
# End of my /etc/init.d/S50non_creality_webcam file
===EOF===

~~~

make the newly created `/etc/init.d/S50non_creality_webcam` file executable
~~~
chmod u+x /etc/init.d/S50non_creality_webcam
~~~



Start the newly created `/etc/init.d/S50non_creality_webcam` service
~~~
/etc/init.d/S50non_creality_webcam restart
~~~
If no video stream was already served by `/opt/bin/mjpg_streamer`, this gives a message because the `killall mjpg_streamer` command does not kill any process. That's normal.
<pre>
root@F005-4A88 /root [#] /etc/init.d/S50non_creality_webcam restart
killall: mjpg_streamer: no process killed
root@F005-4A88 /root [#] 
</pre>


And now you should get a video stream at the address (replace 192.168.1.23 with your printer's IP address on your local network)
~~~
http://192.168.1.23:8080/?action=stream
~~~

and on the “Creality Print” web interface:
~~~
http://192.168.1.23/#/home
~~~

and (only if nginx, moonraker and mainsail are already installed?)
~~~
http://192.168.1.23:4409/webcam/?action=stream
~~~

It's normal to get an error message at the URL [http://192.168.1.23:8080/](http://192.168.1.23:8080/) because the full URL isn't used
<pre>
501: Not Implemented!
no www-folder configured
</pre>


Under the mainsail web interface ([http://192.168.1.23:4409](http://192.168.1.23:4409)) if installed, you may need to create, or delete and recreate, a webcam with the following configuration:
 - streaming URL
~~~
http://192.168.1.23:4409/webcam/?action=stream
~~~
 - snapshot URL
~~~
http://192.168.1.23:4409/webcam/?action=snapshot
~~~

---

---

Commands to use in case of issues

~~~
v4l2-ctl --all
~~~

~~~
ls -la /opt/lib/mjpg-streamer/
~~~
<details>
 <summary>Command output on my machine (Click to expand!)</summary>
<pre>
root@F005-4A88 /root [#] ls -la /opt/lib/mjpg-streamer/
total 56
drwxr-xr-x    2 root     root          4096 Feb  6 02:09 ./
drwxr-xr-x    6 root     root          4096 Sep  1 21:07 ../
-rw-r--r--    1 root     root         12016 Sep  1 21:07 input_http.so
-rw-r--r--    1 root     root         35760 Sep  1 21:07 output_http.so
root@F005-4A88 /root [#] 
</pre>
</details>

~~~
opkg install v4l-utils
~~~

---


Resources:
 - Some messages from user `destinal` on [https://www.reddit.com/r/Ender3V3KE/](https://www.reddit.com/r/Ender3V3KE/) and on the Discord server [https://discord.gg/d3vil-design](https://discord.gg/d3vil-design)
