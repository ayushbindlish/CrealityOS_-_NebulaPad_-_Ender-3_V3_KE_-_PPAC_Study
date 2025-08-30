# CrealityOS - NebulaPad - Ender-3 V3 KE - PPAC_Study

My notes on firmware v1.1.0.12 (Creality OS) of the Ender-3 V3 KE


> [!WARNING]
> **Work in progress**


> [Rewording of an excerpt from [https://github.com/fran6p/SonicPad/blob/main/Obico/obico-spad.md](https://github.com/fran6p/SonicPad/blob/main/Obico/obico-spad.md)]
> Creality OS, Creality's operating system (derived from OpenWRT), uses neither `systemd` for service startup nor `sudo` to execute commands with root privileges.
> In addition, the versions of Klipper and Moonraker are old and modified to take into account the hardware characteristics of Creality devices (Sonic Pad / Nebula Pad / K1, K1 Max, ...).

> [!WARNING]
> So **do not update Moonraker or Klipper/Klippy even if the Fluidd or Mainsail interfaces offer to do so.**
> Otherwise the machine is very likely to crash and you will need to reset the machine or even recover the firmware.


- My discovery topic of the Ender-3 V3 KE on the lesimprimantes3d.fr forum
  - [https://www.lesimprimantes3d.fr/forum/topic/56116-creality-ender-3-v3-ke-la-d%C3%A9couverte-avant-le-test/#comment-572037](https://www.lesimprimantes3d.fr/forum/topic/56116-creality-ender-3-v3-ke-la-d%C3%A9couverte-avant-le-test/#comment-572037)
- Other topics related to the Ender-3 V3 KE firmware
  - [https://www.lesimprimantes3d.fr/forum/topic/56971-obico-sur-ender-v3-ke/#comment-578771](https://www.lesimprimantes3d.fr/forum/topic/56971-obico-sur-ender-v3-ke/#comment-578771)
  - [https://www.lesimprimantes3d.fr/forum/topic/57003-boulette-suite-maj-ender-3-v3-ke-firmware-11012-root%C3%A9-plantage-suite-mise-a-jour-de-klipper-via-linterface-fluidd/#comment-579230](https://www.lesimprimantes3d.fr/forum/topic/57003-boulette-suite-maj-ender-3-v3-ke-firmware-11012-root%C3%A9-plantage-suite-mise-a-jour-de-klipper-via-linterface-fluidd/#comment-579230)

## Password for .7z log export archives

When you export logs from the Ender-3 V3 KE's Nebula Pad screen, you get a .7z archive protected by a password
example of archive name `2023-12-19_12-57-13_creality_log.7z`

Here is the password

```
b[^8xd6fR$w*N&em
```

Source https://www.reddit.com/r/Ender3V3KE/comments/1ah6vde/comment/kovd3vb/ (thanks to user "slipx06")

In fact the password is hidden in the binary code of `/usr/bin/display-server` but I would never have found it. (For more details see https://www.lesimprimantes3d.fr/forum/topic/56116-creality-ender-3-v3-ke-la-d%C3%A9couverte-avant-le-test/?do=findComment&comment=579689
)

## Official firmware v1.1.0.12 that provides a root mode

- [https://github.com/CrealityOfficial/Ender-3_V3_KE_Klipper/releases/tag/V1.1.0.12](https://github.com/CrealityOfficial/Ender-3_V3_KE_Klipper/releases/tag/V1.1.0.12)


### Activate root mode to then connect via SSH

(adapted from [https://www.lesimprimantes3d.fr/forum/topic/56116-creality-ender-3-v3-ke-la-d%C3%A9couverte-avant-le-test/?do=findComment&comment=576232](https://www.lesimprimantes3d.fr/forum/topic/56116-creality-ender-3-v3-ke-la-d%C3%A9couverte-avant-le-test/?do=findComment&comment=576232) )

To activate root mode :
 * Via the printer's control screen (NebulaPad) cf [https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/blob/main/root%20guide/root%20tutorial.pdf](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex/blob/main/root%20guide/root%20tutorial.pdf)
 * default password : Creality2023


#### Connect via ssh

~~~
ssh <ip> -l root
~~~
after accepting the host key and entering the root password
~~~
//TODO
~~~

Or use for example MobaXterm (Windows) or SnowFlake (linux). (port 22 ( = ssh), user `root` default password `Creality2023`)

( Note that sftp is not installed by default in CrealityOS, so initially ( unless after having installed `entware` which allows the `opkg` package manager to be set up to install `sftp` on the KE) it will not be possible to explore the file tree but only to use the ssh console mode. )



( a good practice is to change the root password, with the command
~~~
passwd
~~~
and write down the new password so you don't lose it
)

---


~~~
cat /usr/data/creality/userdata/config/system_version.json | jq -r '.sys_version'
~~~
returns
<pre>
V1.1.0.12
</pre>

---

~~~
df -h | grep /dev/mmcblk0p10 | awk {'print $3 " / " $2 " (" $4 " available)" '}
~~~
returns something like
<pre>
2.6G / 5.9G (3.0G available)
</pre>


## Reference repositories

- Official from Creality (the NebulaPad uses firmware based on Creality OS)
  - [https://github.com/CrealityOfficial/Ender-3_V3_KE_Klipper](https://github.com/CrealityOfficial/Ender-3_V3_KE_Klipper)
  - [https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex](https://github.com/CrealityOfficial/Ender-3_V3_KE_Annex)
- Sonic Pad firmware and/or the K1 (based on Creality OS)
  - [https://github.com/fran6p/SonicPad](https://github.com/fran6p/SonicPad)
  - [https://github.com/Guilouz/Creality-K1-and-K1-Max](https://github.com/Guilouz/Creality-K1-and-K1-Max)
    - [https://github.com/Guilouz/Creality-K1-and-K1-Max/wiki/Helper-Script-Installation](https://github.com/Guilouz/Creality-K1-and-K1-Max/wiki/Helper-Script-Installation)https://github.com/Guilouz/Creality-K1-and-K1-Max/wiki/Helper-Script-Installation
      - ~~~
        cd && wget --no-check-certificate https://raw.githubusercontent.com/Guilouz/Creality-K1-and-K1-Max/main/Scripts/installer.sh
        cd && sh ./installer.sh
        ~~~
      - [https://github.com/Guilouz/Creality-K1-and-K1-Max/blob/main/Scripts/installer.sh](https://github.com/Guilouz/Creality-K1-and-K1-Max/blob/main/Scripts/installer.sh)


