

To have the web server serve the file `/usr/data/creality/userdata/history/print_history_record.json` via `http://<ip>/downloads/humbnail/historyL.txt`

Creating a service to make a symbolic link for the file we want to expose through the pre-installed web server

Note that the symbolic link must be created after the `/etc/init.d/S99start_app` service starts, which launches the application that generates and keeps the file `/usr/data/creality/userdata/history/print_history_record.json` up to date.
Hence the name `S99t_hist_http`, alphabetically after `S99start_app`.)

~~~
cat > /etc/init.d/S99t_hist_http<<'===EOF==='
#!/bin/sh
#

case "$1" in
  start)
        ln -s /usr/data/creality/userdata/history/print_history_record.json /tmp/creality/local_gcode/humbnail/historyL.txt
        ;;
  stop)
        ;;
  restart|reload)
        ;;
  *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
esac

exit $?
===EOF===

~~~

Note that putting the line identifying the end of the block in quotes at the beginning prevents special characters and variables from being interpreted. Make sure the body of the block does not contain a line identical to the one used to mark the end of the block (quoted at the start here). See [https://stackoverflow.com/questions/6896025/echo-a-large-chunk-of-text-to-a-file-using-bash](https://stackoverflow.com/questions/6896025/echo-a-large-chunk-of-text-to-a-file-using-bash).

make the created file executable
~~~
chmod a+x /etc/init.d/S99t_hist_http
~~~

and finally

start the service
~~~
/etc/init.d/S99t_hist_http start
~~~

or restart/reboot the system
~~~
reboot
~~~

(Normally, at each startup, the symbolic link will be created by this service. Therefore, the content of `/usr/data/creality/userdata/history/print_history_record.json` is available at `http://<ip>/downloads/humbnail/historyL.txt`.)

On my machine, it's at [http://192.168.1.23/downloads/humbnail/historyL.txt](http://192.168.1.23/downloads/humbnail/historyL.txt)










