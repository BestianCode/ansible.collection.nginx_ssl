# The Ansible collection with a set of roles for configuring and managing Nginx servers and LetsEncrypt SSL certificates.

## Installation

You can install this collection using the `ansible-galaxy` command:

```bash
ansible-galaxy collection install bestiancode.nginx_ssl
```

Alternatively, you can create a `requirements.yml` file and use it to install all collections and roles at once:

```bash
ansible-galaxy collection install -r requirements.yml --force
ansible-galaxy role install -r requirements.yml --force
```

Here's a sample `requirements.yml` file:

```yaml
collections:
  - name: bestiancode.sysadmin
  - name: bestiancode.nginx_ssl

roles:
  - name: geerlingguy.docker
  - name: newrelic.newrelic_install
```

## Usage

And a sample `playbook`:

```yaml
- hosts:
    - web
  become: true
  tags:
    - nginx
  roles:
    # If collections are not defined here, it is mandatory to specify prefix bestiancode.nginx_ssl!
    - bestiancode.nginx_ssl.install
    - bestiancode.nginx_ssl.letsencrypt
    - bestiancode.nginx_ssl.reload
    # Add other roles here

- hosts:
    - web
  become: true
  tags:
    - nginx
  collections:
    # If collections are defined here...
    - bestiancode.nginx_ssl
  roles:
    # If you defined collections, prefix bestiancode.nginx_ssl is not needed.
    - install
    - letsencrypt
    - reload
    # Add other roles here
```

## URLs

- **GitHub**: https://github.com/BestianCode/ansible.collection.nginx_ssl
- **Galaxy**: https://galaxy.ansible.com/ui/repo/published/bestiancode/nginx_ssl
