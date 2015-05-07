# PHP5-FPM Role

## Variables
- `php5_fpm_pools` - List of pools to create. Each should contain:
  - `name` e.g. my_service
  - `prefix` e.g. `/var` _default:_ not set
  - `user` - user to run the pool process as; _default:_ `www-data`
  - `group` - group to run the pool process as; _default:_ `www-data`
  - `socket_listen_owner` e.g. nginx _default:_ www-data
  - `socket_listen_group` e.g. nginx _default:_ www-data
  - `socket_listen_mode` e.g. 0600 _default:_ not set
  - `pm_mode` - one of `static`, `dynamic` or `ondemand` _default:_ `dynamic`
  - `max_children` _default:_ `5`
  - `min_spare_servers` _default:_ `1`
  - `max_spare_servers` _default:_ `3`
  - `process_max_requests` _default:_ `500`
  - `access_log_path` e.g. `/var/log/$pool.access.log`
  - `status_path` _default:_ not set

> NOTE: make sure the user specified in `user`, `group` and `socket_listen_owner` and `socket_listen_group` already exist

- `php_flags` - Additional php flags e.g. `php_value[session.save_path] = /var/php/sessions` or `php_admin_value[session.save_path] = /var/php/sessions` to stop the value from being overwritten by PHP call 'ini_set'
