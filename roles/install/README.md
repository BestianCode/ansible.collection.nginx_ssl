# Role Description

Ansible role for initial installation of Nginx server

## Default Variables

For a comprehensive understanding of default variables, please refer to the `defaults/main.yml` file.

## Logrotate configuration

The role can manage `/etc/logrotate.d/nginx` via variables:

- `nginx_logrotate_enabled`
- `nginx_logrotate_frequency` (`daily`, `hourly`, `weekly`, ...)
- `nginx_logrotate_size` (native `size` directive, e.g. `500M`)
- `nginx_logrotate_rotate`
- `nginx_logrotate_extra_options` (for directives like `maxsize 500M`)

This role now keeps logrotate management native only (no dedicated cron scheduling).
It also removes legacy cron entries and legacy standalone config files from older role versions.

## logrotate.timer schedule

The role can also manage systemd `logrotate.timer` override to run logrotate twice per day.

- `nginx_logrotate_timer_manage`
- `nginx_logrotate_timer_on_calendar` (default `*-*-* 00,12:00:00`)
- `nginx_logrotate_timer_randomized_delay_sec` (default `0`)
- `nginx_logrotate_timer_accuracy_sec` (default `1min`)

Example: rotate daily and by size

```yaml
nginx_logrotate_frequency: daily
nginx_logrotate_size: 1024M
```

Example: run logrotate timer twice per day

```yaml
nginx_logrotate_timer_manage: true
nginx_logrotate_timer_on_calendar: "*-*-* 00,12:00:00"
nginx_logrotate_timer_randomized_delay_sec: "0"
nginx_logrotate_timer_accuracy_sec: "1min"
```
