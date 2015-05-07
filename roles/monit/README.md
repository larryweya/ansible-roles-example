# Monit Role

## Variables
- `monit_interval` - Monitoring interval _default:_ 120
- `monit_start_delay` - Time in seconds that monit should delay the first service check _optional_ _default_ not set If not defined it will not be set
- `monit_httpd_port` - Whether to enable the monit http daemon and the port to bind to to _default:_ 2812
- `monit_httpd_allow_connect` - List of hosts allowed to connect to the http daemon _default:_ localhost
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
