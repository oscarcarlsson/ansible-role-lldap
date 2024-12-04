# danielsreichenbach.lldap

[![CI][badge-img]][badge-url]
[![Galaxy Role][galaxy-img]][galaxy-url]

An Ansible role for installing and configuring [Light LDAP][] on Debian and
Ubuntu servers.

It is recommended to use a proxy for the web interface.

## Requirements

- Optional: to run Light LDAP behind a proxy you may something like
  `geerlingguy.nginx` to deploy `nginx`.

Other proxies such as Caddy or Traefik or fine too.

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):

```yaml
lldap_repo_enable: true
lldap_repo_filename: "lldap"
lldap_repo_origin: "download.opensuse.org"
lldap_packages: ~ # distro specific
lldap_repo_line: ~ # distro specific

lldap_conf: "/etc/lldap"
lldap_data_dir: "/var/lib/lldap"
lldap_system_service: "/lib/systemd/system/lldap.service"

lldap_group: "lldap"
lldap_user: "lldap"

lldap_ldap_host: "127.0.0.1"
lldap_ldap_port: 3890
lldap_http_host: "127.0.0.1"
lldap_http_port: 17170
lldap_http_url: "http://localhost:17170"
lldap_jwt_secret: "REPLACE_WITH_RANDOM"
lldap_ldap_base_dn: "dc=example,dc=com"
lldap_ldap_user_dn: "admin"
lldap_ldap_user_email: "admin@example.com"
lldap_ldap_user_pass: "REPLACE_WITH_PASSWORD"
lldap_force_reset_admin_password: false
lldap_key_seed: "RanD0m STR1ng"

# by default SQLite3 is used, but any supported backend should work
lldap_database_url: "sqlite://{{ default_lldap_data_dir }}/users.db?mode=rwc"
```

Light LDAP can be configured to handle emails for accounts too, when
`lldap_smtp_enable` is `true`.

```yaml
lldap_smtp_enable: false
lldap_smtp_enable_password_reset: false
lldap_smtp_server: "smtp.example.com"
lldap_smtp_port: "587"
lldap_smtp_smtp_encryption: "TLS"
lldap_smtp_user: "sender@example.com"
lldap_smtp_password: "password"
lldap_smtp_from: "LLDAP Admin <sender@example.com>"
lldap_smtp_reply_to: "Do not reply <noreply@localhost>"
```

If you want to expose LDAPS, set `lldap_ldaps_enable` to `true` and set
`lldap_ldaps_private_key` & `lldap_ldaps_certificate` to point to their
respective file names.

```yaml
lldap_ldaps_enable: false
lldap_ldaps_port: 6360
#lldap_ldaps_private_key: "path to private key"
#lldap_ldaps_certificate: "path to server certificate"
```

If you want to bind the service to lower TCP ports (<1024), like 389 & 689,
set below to `true`:

```yaml
lldap_bind_lower_ports: false
```

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - danielsreichenbach.lldap
```

## License

MIT

## Author Information

This role has received contributions from

- [danielsreichenbach](https://github.com/danielsreichenbach)

[Light LDAP]: https://github.com/lldap/lldap
[badge-img]: https://github.com/danielsreichenbach/ansible-role-lldap/workflows/CI/badge.svg?event=push
[badge-url]: https://github.com/danielsreichenbach/ansible-role-lldap/actions?query=workflow%3ACI
[galaxy-img]: https://img.shields.io/badge/ansible--galaxy-lldap-blue.svg
[galaxy-url]: https://galaxy.ansible.com/danielsreichenbach/lldap/
