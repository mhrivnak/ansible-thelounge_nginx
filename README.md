Role Name
=========

Exposes The Lounge IRC webapp using nginx as a reverse proxy and [Let's
Encrypt](https://letsencrypt.org/) for TLS certs. This role assumes that The
Lounge is already running and available on localhost.

Requirements
------------

You must ensure that The Lounge is already running; this role only sets up a
reverse proxy using nginx.

Here is a role that is recommended for deploying The Lounge with podman:
https://github.com/mhrivnak/ansible-thelounge_podman

You also must setup a DNS record for `thelounge_fqdn` so that the service is
accessible, and so `certbot` can obtain a TLS certificate.

Role Variables
--------------

`thelounge_fqdn`: the fully qualified domain name where The Lounge will be accessible.
`thelounge_port`: the port on localhost where The Lounge is deployed and listening.

Dependencies
------------

None

Example Playbook
----------------

The following playbook will deploy The Lounge:
- as a rootless container using podman
- with nginx as a reverse proxy
- with a TLS certificate from [letsencrypt](https://letsencrypt.org/)

```
- hosts: irc_client_host
  become: true

  roles:
    - mhrivnak.nginx
    - mhrivnak.thelounge_podman
    - geerlingguy.certbot
    - mhrivnak.thelounge_nginx

  vars:
    certbot_admin_email: certs@example.com
    certbot_create_if_missing: true
    certbot_certs:
      - domains:
          - irc.example.com
    thelounge_fqdn: irc.example.com
    thelounge_port: 9000
```

License
-------

GPLv3