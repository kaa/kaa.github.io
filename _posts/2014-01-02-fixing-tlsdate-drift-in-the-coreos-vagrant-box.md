---
layout: post
title: Fixing tlsdate drift in the CoreOS vagrant box
description: ""
category: 
tags: []
---

The CoreOS vagrant image is a great way to get started quickly playing with docker. However, I find that when I suspend the host computer and bring it back up again with the VM running, the clock in the CoreOS VM has drifted. I figure this is because there are no guest additions in the VM which would otherwise synchronize the time automatically. Here is how to fix it with a `systemd` timer. 

My first thought was to update `/etc/tlsdate/tlsdated.conf` and make it synchronize the clock more often. As configured `tlsdated` will synchronize time every 24h once it reaches a steady state. The problem is that in the CoreOS system the `/etc` directory is mapped readonly so it is not possible to change the configuration. My second thought was to just create a `cron` task to poll the time every 5 minutes using the `tlsdate` client, unfortunately CoreOS doesn't seem to come with `cron` installed so that is a dead end.

Here is how I used `systemd` to schedule a periodic polling of the current date using the `tlsdate` client instead. I learned how from [Jason's blog post about creating timers](http://jason.the-graham.com/2013/03/06/how-to-use-systemd-timers/).

First I created a service `/media/state/units/tlsdatesync.service`.

```ini
[Unit]
Description=Synchronize time with tlsdate

[Service]
Type=simple
ExecStart=/usr/bin/tlsdate
```

and a timer description, `/media/state/units/tlsdatesync.timer`

```ini
[Unit]
Description=Run tlsdate every five minutes

[Timer]
Unit=tlsdatesync.service
# Time to wait after booting before we run first time
OnBootSec=10min
# Time between running each consecutive time
OnUnitActiveSec=5min

[Install]
WantedBy=local.target
```

then I configured the service and timers, and enable the timer on startup,

```bash
% sudo systemctl restart local-enable.service
% sudo systemctl enable --runtime /media/state/units/tlsdatesync.timer
```

I checked the timers status with `systemctl` which confirmed the timer was running

```bash
% systemctl status tlsdatesync.timer
tlsdatesync.timer - Runs tlsdated every five minutes
   Loaded: loaded (/media/state/units/tlsdatesync.timer; enabled-runtime)
   Active: active (waiting) since Wed 2014-01-01 22:47:23 UTC; 10min ago

Jan 01 22:47:23 localhost systemd[1]: Starting Runs tlsdated every five minutes.
Jan 01 22:47:23 localhost systemd[1]: Started Runs tlsdated every five minutes.
```