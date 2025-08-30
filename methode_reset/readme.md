# Reset Method

The K1* reset method https://github.com/Guilouz/Creality-K1-and-K1-Max/wiki/Reset-Factory-Settings seems to work for the Ender-3 V3 KE.

(

Too bad it seems we absolutely have to have the page.html locally for it to work...


I took the liberty of putting Gilouz's reset page `Creality_K1_Reset_Utility.html` online via github.io at the address

https://ppac37.github.io/CrealityOS_-_NebulaPad_-_Ender-3_V3_KE_-_PPAC_Study/methode_reset/Creality_K1_Reset_Utility.html

So you don't have to download the [Creality_K1_Reset_Utility.zip](https://github.com/Guilouz/Creality-K1-and-K1-Max/raw/main/Scripts/Creality_K1_Reset_Utility.zip), unzip it, to obtain the `Creality_K1_Reset_Utility.html` that performs the reset (an embedded js script sends a `{"method":"set","params":{"resetSystem":15}}` to the printer's web socket on port 9999).

A big thank-you to https://github.com/Guilouz

)

---

https://discord.com/channels/1154500511777693819/1186800299424366643/1207072102369202186

~~~
echo "all" | nc -U /var/run/wipe.sock
~~~

---

https://discord.com/channels/1154500511777693819/1186800299424366643/1207080347817484338

just be aware, it triggers a reboot automatically and deletes EVERYTHING

if you want to do the equivalent of the factory reset that is initiated from the k1 screen you can use this command:

~~~
echo "part" | nc -U /var/run/wipe.sock
~~~

I have posted above what each of them actually does in terms of deletions

---

https://discord.com/channels/1154500511777693819/1186800299424366643/1207203706500550676

~~~
echo "part" | nc -U /var/run/wipe.sock
~~~
=
~~~
/bin/rm called with args: -rf /usr/data/wpa_supplicant.conf
/bin/rm called with args: -rf /usr/data/ai_image
/bin/rm called with args: -rf /usr/data/timelapse
/bin/rm called with args: -rf /usr/data/lost+found
/bin/rm called with args: -rf /overlay/upper/*
~~~
