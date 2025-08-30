
---

To create a `/root/.profile` file so that at session startup the available disk space and some additional information are displayed
(`~/.profile` is automatically executed when a session starts)
~~~
cat > /root/.profile<<'===EOF==='
# My ~/.profile file automatically executed when a session starts

# Display the space used by .gcode files and logs
du -hc /usr/data/printer_data/gcodes/ /usr/data/printer_data/logs/ /usr/data/creality/userdata/log
echo -e "\n"

# Alias definitions
# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# network connections
#ifconfig
#echo -e "\n"

# listening ports
#netstat -tunle
#echo -e "\n"

###
# The following blocks (possibly adapted) of code were taken from https://github.com/Guilouz/Creality-K1-and-K1-Max/blob/main/Scripts/installer.sh
# Thanks Guilouz!


# The Creality OS version
echo "CrealityOS `cat /usr/data/creality/userdata/config/system_version.json | jq -r '.sys_version'`"

# Show available disk space
df -h | grep /dev/mmcblk0p10 | awk {'print $3 " / " $2 " (" $4 " available)" '}


#model=`/usr/bin/get_sn_mac.sh model`

white=`echo -en "\033[m"`
blue=`echo -en "\033[36m"`
cyan=`echo -en "\033[1;36m"`
yellow=`echo -en "\033[1;33m"`
green=`echo -en "\033[01;32m"`
darkred=`echo -en "\033[31m"`
red=`echo -en "\033[01;31m"`

printer_config="/usr/data/printer_data/config/printer.cfg"
macro_config="/usr/data/printer_data/config/gcode_macro.cfg"
klipper_extra_folder="/usr/share/klipper/klippy/extras/"
firmware_version="$(cat /usr/data/creality/userdata/config/system_version.json | jq -r '.sys_version')"

moonraker_folder="/usr/data/moonraker/"
nginx_folder="/usr/data/nginx/"
fluidd_folder="/usr/data/fluidd"

mainsail_folder="/usr/data/mainsail/"




check_folder() {
    local folder_path="$1"
    if [ -d "$folder_path" ]; then
        printf "${green}✓"
    else
        printf "${red}✗"
    fi
}


hr() {
    printf ' %s\n' '│                                                              │'
}


infoline() {
    local status=$1
    local text=$2
    local color=$3
    local max_length=66
    local total_text_length=$(( ${#status} + ${#text} ))
    local padding=$((max_length - total_text_length))
    printf " │   $color${status} ${white}${text}%-${padding}s${white}│ \n" ''
}


check_file() {
    local file_path="$1"
    if [ -f "$file_path" ]; then
        printf "${green}✓"
    else
        printf "${red}✗"
    fi
}

check_ipaddress() {
    eth0_ip=$(ip -4 addr show eth0 2>/dev/null | grep -o -E '(inet\s)([0-9]+\.){3}[0-9]+' | cut -d ' ' -f 2 | head -n 1)
    wlan0_ip=$(ip -4 addr show wlan0 | grep -o -E '(inet\s)([0-9]+\.){3}[0-9]+' | cut -d ' ' -f 2 | head -n 1)
    if [ -n "$eth0_ip" ]; then
        printf "$eth0_ip"
    elif [ -n "$wlan0_ip" ]; then
        printf "$wlan0_ip"
    else
        printf "xxx.xxx.xxx.xxx"
    fi
}

check_ipaddress
echo -e "\n"

check_connection() {
    eth0_ip=$(ip -4 addr show eth0 2>/dev/null | grep -o -E '(inet\s)([0-9]+\.){3}[0-9]+' | cut -d ' ' -f 2 | head -n 1)
    wlan0_ip=$(ip -4 addr show wlan0 | grep -o -E '(inet\s)([0-9]+\.){3}[0-9]+' | cut -d ' ' -f 2 | head -n 1)
    if [ -n "$eth0_ip" ]; then
        printf "$eth0_ip (ETHERNET)"
    elif [ -n "$wlan0_ip" ]; then
        printf "$wlan0_ip (WLAN)"
    else
        printf "xxx.xxx.xxx.xxx"
    fi
}

check_version() {
  file="/usr/data/creality/userdata/config/system_version.json"
  if [ -e "$file" ]; then
    cat "$file" | jq -r '.sys_version'
  else
    printf "N/A"
  fi
}

##
#    infoline "$(check_folder "$moonraker_folder")" 'Moonraker'
    infoline "$(check_folder "$moonraker_folder")" $moonraker_folder

  ## Nginx = web server,
# the ports for the servers are defined in /usr/data/nginx/nginx/nginx.conf
# cat /usr/data/nginx/nginx/nginx.conf
#    infoline "$(check_folder "$nginx_folder")" 'Nginx'
    infoline "$(check_folder "$nginx_folder")" $nginx_folder

##
#    infoline "$(check_folder "$fluidd_folder")" 'Fluidd :4408'
    infoline "$(check_folder "$fluidd_folder")" "$fluidd_folder :4408"

##
#    infoline "$(check_folder "$mainsail_folder")" 'Mainsail :4409'
    infoline "$(check_folder "$mainsail_folder")" "$mainsail_folder :4409"

#    hr

# End of my ~/.profile file
===EOF===

~~~

---

