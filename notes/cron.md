

# Entry every weekday at 8:45am

45 8 * * 1-5 python command.py

# Set up cron logs

Open the file
```
/etc/rsyslog.d/50-default.conf
```
Find the line that starts with:
```
#cron.*
```
Uncomment that line, save the file, and restart rsyslog:
```
sudo service rsyslog restart
```
You should now see a cron log file here:
```
/var/log/cron.log
```
