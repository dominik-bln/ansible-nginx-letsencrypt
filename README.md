# Ansible Nginx Let's Encrypt

An Ansible role to install nginx on Ubuntu based on
[h5bp's server configuration templates](https://github.com/h5bp/server-configs-nginx).
It offers simple configuration of SSL hosts with the ability to use [Let's Encrypt](https://letsencrypt.org/)
for the creation and renewal of free SSL certificates.

Hosts can be created with a simple dictionary as shown below.

Install via ansible-galaxy:

```bash
$ ansible-galaxy install dominik-bln.nginx-letsencrypt
```

## Role Variables

The most important configuration is the `nginx_domains`dictionary where all host configuration is defined. Below shows an example with all possible options and their respective defaults when nothing is set. The only actually required value is `admin_email` as long as `letsencrypt`is set to true.

```yml
nginx_domains:
  example.com:
    # create a certificate with letsencrypt
    letsencrypt: true
    # email used for registering Let's Encrypt certificate
    admin_email: admin@example.com
    # create a virtual host file in sites-enabled
    create_vhost: true
    # symlink to sites-enabled if the host file exists
    enabled: true
    # create a htdocs directory
    create_htdocs: true
    # create a simple index file in the htdocs
    create_index: false
    # redirect all http traffic to https
    http_redirect: true
    # additional aliases to redirect; separate with whitespace
    redirects: www.example.com example.org
    # specify the ssl_certifcate explicitly
    ssl_certificate: false
    # specify the ssl_certificate_key file
    ssl_certificate_key: false
    # specify the ssl_trusted_certificate file directly
    trusted_cert: false
```

A lot of other paths and switches can be changed if necessary. Please consult `defaults/main.yml`to see what is possible.

## Example Playbook

```yml
---
- hosts: webservers
  roles:
    - { role: dominik-bln.nginx-letsencrypt }
```

## Requirements

The role is only tested on Ubuntu 14.04 and with Ansible 1.9. There is a chance
that it would work for other versions as well (testing would be very welcome).

## Roadmap

* crontab for renewal of Let's encrypt certificates
* travis.ci tests
* custom nginx include file
* replaceable nginx template on a host basis

## License

This project is licensed under an MIT license. It includes code from
[h5bp/server-config-nginx](https://github.com/h5bp/server-configs-nginx) for the
configuration of nginx.
