# Role Description

Ansible role for installation and configuration of haproxy server

## Default Variables

For a comprehensive understanding of default variables, please refer to the `defaults/main.yml` file.

## Logrotate configuration

The role can manage `/etc/logrotate.d/haproxy` via variables:

- `haproxy_logrotate_enabled`
- `haproxy_logrotate_frequency` (`daily`, `hourly`, `weekly`, ...)
- `haproxy_logrotate_rotate`
- `haproxy_logrotate_extra_options` (for directives like `maxsize 500M`)

For sub-daily schedules, enable a dedicated cron entry that runs logrotate for HAProxy only:

- `haproxy_logrotate_cron_enabled`
- `haproxy_logrotate_cron_hour`
- `haproxy_logrotate_cron_minute`

Example: rotate hourly

```yaml
haproxy_logrotate_frequency: hourly
haproxy_logrotate_cron_enabled: true
haproxy_logrotate_cron_hour: "*"
haproxy_logrotate_cron_minute: "5"
```

Example: rotate every 6 hours

```yaml
haproxy_logrotate_frequency: hourly
haproxy_logrotate_cron_enabled: true
haproxy_logrotate_cron_hour: "*/6"
haproxy_logrotate_cron_minute: "5"
```
