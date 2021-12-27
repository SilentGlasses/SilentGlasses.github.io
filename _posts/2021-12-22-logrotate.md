---
title: Logrotate
author:
  name: SilentGlasses
  link: https://github.com/SilentGlasses
date: 2021-12-22 07:00:00
categories: [Administration, Utilities]
tags: [utilities, linux, servers, administration]
---

Logrotate is a tool admins use to manage system logs on their Linux servers.

Config files are kept in `/etc/logrotate.d/`, to add a new file you would want to do the following, changing `config_file` to a proper name:

```
touch /etc/logrotate.d/config_file
```

```
chmod 644 /etc/logrotate.d/config_file
```

```
chown root.root /etc/logrotate.d/config_file
```

Then edit the file with your required settings.

To ensure your config changes are free of errors, you can run this command:

```
logrotate -f /etc/logrotate.d/config_file  
```

Some things you can do with logrotate are:

* rotate our logs daily, weekly, monthly
* rotate your logs based on file size
* compress log files after rotation
* remove old rotated logs

## Logrotate Configuration files

* `/usr/sbin/logrotate` – The logrotate command itself.
* `/etc/cron.daily/logrotate` – This shell script executes the logrotate command everyday.
* `/etc/logrotate.conf` – Log rotation configuration for all the log files are specified in this file.
* `/etc/logrotate.d` – When individual packages are installed on the system, they drop the log rotation configuration information in this directory.
