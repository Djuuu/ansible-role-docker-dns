Ansible Role: Docker-dns
========================

Install docker-dns Docker Compose Project:  
automatic container DNS for Docker in a single Python file.

- https://github.com/phensley/docker-dns

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

docker_dns_project_name: docker-dns

# docker-dns project variables

docker_dns_version: latest

docker_dns_domain: example.net
```

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: false
  roles:
    - djuuu.docker_dns
```

Example usage
-------------

Identify docker clients in [Pi-hole](https://pi-hole.net/) using docker-dns for reverse DNS resolution

* Assign a specific IP subnet to docker-dns

  ex: using _`{{ docker_project_prefix }}`_**`_network_ipv4_subnet`** [dynamic variable](https://github.com/Djuuu/ansible-role-docker-project#dynamic-variables)

  `host_vars/example/docker_dns.yml`: 
  ```yaml
  docker_dns_network_ipv4_subnet: 172.xx.0.0/16
  ```
  (Container will usually take IP `172.xx.0.2`)

* Configure reverse DNS resolution with docker-dns for Docker IP ranges in Pi-hole's Dnsmasq configuration

  ex: using [`djuuu.pihole_docker` additional config files](https://github.com/Djuuu/ansible-role-pihole-docker#additional-config-files):

  `config/pihole/{{ inventory_hostname }}/etc-dnsmasq.d/04-docker-resolving.conf`
  ```ini
  ## Resolve docker IP ranges through docker-dns container
  
  # Docker v4 ranges (172.16.0.0 – 172.31.255.255)
  rev-server=172.16.0.0/12,172.xx.0.2
  ```

License
-------

Beerware License
