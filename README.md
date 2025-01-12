Docker-install
==============

Install/update `docker` and `docker-compose` to Ubuntu/debian servers.
Also install pip dependancies `docker` and `docker-compose` for Ansible.
Installed aliases file that can be use in `.bash_rc` manually with `source /usr/share/.bash_aliases.d/docker.aliases` or `source ~/.bash_aliases.d/docker.aliases` according to the `docker_bash_alias_shared` variable. It can be used automatically with the [basic role](https://galaxy.ansible.com/belgotux/basic) ([Github](https://github.com/belgotux/ansible-role-basic))

Requirements
------------

- Ansible collection community.docker ([documentation](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_compose_module.html)) : `ansible-galaxy collection install community.docker`

Role Variables
--------------
The role can work as it with the [default configuration](defaults/main.yml).

### Needed
- `docker_data_path` path you choose to put persistent data

### Optional
- `docker_extra_users` list of users who need to run docker
- `docker_bash_alias` install docker and docker-compose aliases (default `true`)
- `docker_bash_alias_shared` enable shared alias (Installed alias in /usr/share only with root user ONLY via `remote_user` or `become` in your playbook) (default no to install only for `remote_user` in his homepath)
- `docker_bash_alias_dir_share`: (default /usr/share)
- `docker_networks` list of externel network to add in this format :
```yml
   - name: proxy-net
   subnet: 10.10.11.0/24
   gateway: 10.10.11.1
```
- `docker_path` specified root directory of docker for settings rights (default /var/lib/docker)
- `docker_volumes` specified a specifique directory for volumes created on the fly. It will move /var/lib/docker/volumes to this folder and make a link.
- `docker_data_path` path you choose to put persistent data
- `docker_granded_group_to_data` group to give access to
- `docker_granded_read_paths` list of paths to give read access
- `docker_granded_write_paths` list of paths to give write access
- `docker_internal_dns` list if dns use into docker containers (default `["1.1.1.1","1.0.0.1"]`)
- `docker_get_host_dns` apply the same DNS of the host into containers and not the docker_internal_dns. Need the file path, see bellow (default `false`)
- `docker_netplan_file`: netplan file to check
- `docker_log_driver`: change docker driver (default `local`)
- `docker_log_max_size`: file size (default `10m`)
- `docker_log_max_files`: number of log files (default `10`)
- `docker_compose_install`: Install docker compose (default `true`)
- `docker_compose_use_repository`: Install the docker compose plugin (true) or standalone (false) (default standalone `false`)
- `docker_compose_version`: standalone version to install (default `1.29.2`)
- `docker_apt_key`: key id and url (default official docker.com)
- `docker_apt_repo`: apt line to docker repo (default official docker.com)

- `docker_swarm` activate docker swarm for network and Traefik mode (default no)

### Automatic
- `docker_bash_alias_dir` generate on the `docker_bash_alias_shared` flag

Example Playbook
----------------
### as root
```yml
- hosts: all
  remote_user: root

  roles:
    - name: docker
      vars:
        docker_bash_alias_shared: yes
        docker_networks:
        - name: proxy-net
          subnet: 10.10.11.0/24
          gateway: 10.10.11.1
        - name: dns-net
          subnet: 10.10.12.0/24
          gateway: 10.10.12.1
        docker_extra_users:
        - belgotux
```

### as a user (become is used in main.yml tasks)
```yml
- hosts: u1
  remote_user: belgotux

  roles:
    - name: docker
      vars:
        docker_bash_alias_shared: no
        docker_networks:
        - name: proxy-net
          subnet: 10.10.11.0/24
          gateway: 10.10.11.1
        docker_extra_users:
        - belgotux
```

License
-------

[GPL v3](https://www.gnu.org/licenses/gpl-3.0.en.html)

Author Information
------------------

Belgotux
[MonLinux](https://www.monlinux.net)