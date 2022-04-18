Docker-install
==============

Install/update `docker` and `docker-compose` to Ubuntu/debian servers.
Also install pip dependancies `docker` and `docker-compose` for Ansible.
Installed aliases file that can be use in `.bash_rc` manually with `source /usr/share/.bash_aliases.d/docker.aliases` or `source ~/.bash_aliases.d/docker.aliases` according to the `bash_alias_shared` variable. It can be used automatically with the [basic role](https://galaxy.ansible.com/belgotux/basic) ([Github](https://github.com/belgotux/ansible-role-basic))

Requirements
------------

- Ansible collection community.docker ([documentation](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_compose_module.html)) : `ansible-galaxy collection install community.docker`

Role Variables
--------------
The role can work as it with the [default configuration](defaults/main.yml).

### Optional
- `extra_users` list of users who need to run docker
- `bash_alias_shared` enable shared alias (Installed alias in /usr/share only **with root user ONLY** via `remote_user` or `become` in your playbook) (default no to install only for `remote_user` in his homepath)
- `bash_alias_dir_share`: (default /usr/share)
- `networks` list of externel network to add in this format :
```yml
   - name: proxy-net
   subnet: 10.10.11.0/24
   gateway: 10.10.11.1
```

### Automatic
- `bash_alias_dir` generate on the `bash_alias_shared` flag

Example Playbook
----------------
### as root
```yml
- hosts: clusters
  remote_user: root

  roles:
    - name: docker
      vars:
        bash_alias_shared: yes
        networks:
        - name: proxy-net
          subnet: 10.10.11.0/24
          gateway: 10.10.11.1
        - name: dns-net
          subnet: 10.10.12.0/24
          gateway: 10.10.12.1
        extra_users:
        - belgotux
```

### as a user (become is used in main.yml tasks)
```yml
- hosts: u1
  remote_user: belgotux

  roles:
    - name: docker
      vars:
        bash_alias_shared: no
        networks:
        - name: proxy-net
          subnet: 10.10.11.0/24
          gateway: 10.10.11.1
        extra_users:
        - belgotux
```

License
-------

[GPL v3](https://www.gnu.org/licenses/gpl-3.0.en.html)

Author Information
------------------

Belgotux
[MonLinux](https://www.monlinux.net)