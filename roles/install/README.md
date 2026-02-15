# Role Description

Ansible role for initial installation of Nginx server

## Default Variables

For a comprehensive understanding of default variables, please refer to the `defaults/main.yml` file.

## Logrotate configuration

The role can manage `/etc/logrotate.d/nginx` via variables:

- `nginx_logrotate_enabled`
- `nginx_logrotate_frequency` (`daily`, `hourly`, `weekly`, ...)
- `nginx_logrotate_rotate`
- `nginx_logrotate_extra_options` (for directives like `maxsize 500M`)

For sub-daily schedules, enable a dedicated cron entry that runs logrotate for Nginx only:

- `nginx_logrotate_cron_enabled`
- `nginx_logrotate_cron_hour`
- `nginx_logrotate_cron_minute`

Example: rotate hourly

```yaml
nginx_logrotate_frequency: hourly
nginx_logrotate_cron_enabled: true
nginx_logrotate_cron_hour: "*"
nginx_logrotate_cron_minute: "10"
```

Example: rotate every 6 hours

```yaml
nginx_logrotate_frequency: hourly
nginx_logrotate_cron_enabled: true
nginx_logrotate_cron_hour: "*/6"
nginx_logrotate_cron_minute: "10"
```
