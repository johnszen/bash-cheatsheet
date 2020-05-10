# cron

```
# Is cron running
systemctl | grep cron
ps -aux | grep cron

# what are running
crontab -l

# edit
crontab -e

# verifier: https://crontab.guru/

# monitor
tail -f /var/log/cron
```

