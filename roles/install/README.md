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

> **Note:** When the cron job is enabled, the logrotate config is automatically
> placed at `nginx_logrotate_cron_config_path` (default `/etc/logrotate-nginx.conf`)
> instead of `/etc/logrotate.d/nginx`.  This prevents the systemd daily
> `logrotate.timer` from also processing the config, which would conflict with
> the cron schedule and result in only one rotation per day.

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
