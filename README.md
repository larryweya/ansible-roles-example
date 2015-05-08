# Monit Role

## Variables
- `monit_interval` - Monitoring interval _default:_ 120
- `monit_start_delay` - Time in seconds that monit should delay the first service check _optional_ _default:_ not set. If not defined it will not be set
- `monit_mail_servers` - List of mail servers and optional port in order of importance e.g
```yaml
monit_mail_servers:
  - localhost
  - mail.foo.com port 26
```

- `monit_alert_email_defs` - List of alert recipient email addresses with optional filters defined inline _optional_ e.g.
```yaml
monit_alert_email_defs:
  - root@example.com # alert on all events
  - dev@example.com not on { instance, action } # Do not alert on Monit start,stop or performs a user initiated action
  - foo@bar only on { timeout, nonexist } # only alert on timeouts and nonexist events
```

- `mail_format` - Override the default mail format with e.g. a custom from address. Useful when your mail server woun't send from monit@short-host-name and you haven't set your fqdn for some reason
```yaml
mail_format: "from: me@example.com"
```
becomes
```yaml
mail-format { from: me@example.com }
```

- `monit_httpd_port` - Whether to enable the monit http daemon and the port to bind to to _default:_ 2812
- `monit_httpd_connect_hosts` - List of hosts allowed to connect to the http daemon _default:_ localhost
> NOTE: Both monit_httpd_port and monit_httpd_connect_hosts have to be defined for the http daemon to be enabled. of which they are by default

- `monit_httpd_connect_creds` -  Username and password in the from user:password required to connect to the http daemon _optional_ _default:_ not set
- `monit_httpd_connect_groups` - List of groups allowed to connect to the daemon and optional privilege flag  _optional_ e.g
```yaml
monit_httpd_connect_groups:
 - admins
 - vagrant readonly
```

- `monit_manage_service_dir` - Let the role manage the service include dir at /etc/monit/conf.d/ by clearing it on each run
- `monit_services` - List of monit service definitions each with:
  - `name` - Name of the service that becomes the name of the file at /etc/monit/conf.d/**name**
  - `check` - Check definition e.g. _check process php5-fpm with pidfile /var/run/php5-fpm.pid_
  - `lines` - List of definitions for this check i.e. service methods, monitoring mode, tests, alerts ... e.g.
    - start program = "/usr/sbin/service php5-fpm start"
    - stop program = "/usr/sbin/service php5-fpm stop"
    - if failed unixsocket /var/run/php5-fpm.sock then alert

  A full definition could look like this:
  ```yaml
  monit_services:
    - name: php5-fpm
      check: check process php5-fpm with pidfile /var/run/php5-fpm.pid
      lines:
        - start program = "/usr/sbin/service php5-fpm start"
        - stop program = "/usr/sbin/service php5-fpm stop"
        - if failed unixsocket /var/run/php5-fpm.sock then alert
  ```
