Moonraker is an interface/API that allows other web interfaces (such as Fluidd or Mainsail) to communicate with Klipper/Klippy.

The easiest way is to use the installation helper script for the K1 by Guilouz https://github.com/Guilouz/Creality-K1-and-K1-Max/

---


My miscellaneous notes in case I eventually do it without Guilouz's script


excerpt from https://github.com/Guilouz/Creality-K1-and-K1-Max/blob/main/Scripts/installer.sh
<pre>
download_URL="https://raw.githubusercontent.com/Guilouz/Creality-K1-and-K1-Max/main/Scripts/files/"

moonraker_config="/usr/data/printer_data/config/moonraker.conf"
moonraker_config_URL="${download_URL}moonraker/moonraker.conf"
moonraker_folder="/usr/data/moonraker/"
moonraker_URL="${download_URL}moonraker/moonraker.tar"
</pre>

~~~
wget --no-check-certificate https://raw.githubusercontent.com/Guilouz/Creality-K1-and-K1-Max/main/Scripts/files/moonraker/moonraker.tar -O /usr/data/moonraker.tar

wget --no-check-certificate https://raw.githubusercontent.com/Guilouz/Creality-K1-and-K1-Max/main/Scripts/files/moonraker/moonraker.conf -O /usr/data/moonraker.conf

tar -C /usr/data/moonraker -xvf /usr/data/moonraker.tar
~~~
