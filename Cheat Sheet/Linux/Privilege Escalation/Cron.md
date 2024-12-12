## Cron Jobs
```
Locations:
1. /etc/crontab
2. /etc/cron.d
3. /var/spool/cron/crontabs/root

Check if you have write permissions to add new cron jobs, or can utilise existing cron jobs to escalate privileges

grep "CRON" /var/log/syslog : check for existing cron jobs
```