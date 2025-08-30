







##### Installing Mainsail

// excerpt from the [README_en](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/blob/main/mainsail/README_en) of [https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/mainsail](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/mainsail)

<pre>
mainsail version v2.7.1

Instruction

Put the same level directory file ( mainsail ) in the root directory of the U disk

Copy the files ( mainsail.sh and mainsail.tar ) to directory ( /usr/data )
cp /tmp/udisk/sda1/mainsail/* /usr/data/

Install mainsail, moonraker and nginx
/usr/data/mainsail.sh install

unstall mainsail, moonraker and nginx
/usr/data/mainsail.sh unstall

Into mainsail
IP with 4409 port
eg, 192.168.1.1:4409
</pre>

So after retrieving and copying the contents of the folder https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/tree/main/mainsail/mainsail into a `mainsail` folder on a USB drive and connecting the USB drive to the NebulaPad screen

<pre>
md5sum /tmp/udisk/sda1/mainsail/*
8dc3a1ab0f0be82aa00b573233e16797  /tmp/udisk/sda1/mainsail/mainsail.sh
105e1215ba1c5dc6fa824fe33878faa2  /tmp/udisk/sda1/mainsail/mainsail.tar
</pre>

copy the files
~~~
cp /tmp/udisk/sda1/mainsail/* /usr/data/
~~~

run the installation script
~~~
/usr/data/mainsail.sh install
~~~

to uninstall later
~~~
/usr/data/fluidd.sh unstall
~~~



---


