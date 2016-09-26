# Postgresql Server

This is a fork of ansible-city.postgresql

* [ansible.cfg](#ansible-cfg)
* [Installation and Dependencies](#installation-and-dependencies)
* [Tags](#tags)
* [Examples](#examples)

This roles installs Postgresql server. It's not designed for production servers.
It's main purpose is to give developer local postgresql server to play with.




## ansible.cfg

This role is designed to work with merge "hash_behaviour". Make sure your
ansible.cfg contains these settings

```INI
[defaults]
hash_behaviour = merge
```




## Installation and Dependencies

To install this role run `ansible-galaxy install grim-fendango.postgresql` or add
this to your `install_roles.yml`.

```YAML
- src: grim-fendango.postgresql
  version: v1.0
```

and run `ansible-galaxy install -p ./roles -r install_roles.yml`




## Tags

This role uses two tags: **build** and **configure**

* `build` - Installs Postgresql server.
* `configure` - Configures and ensures that the service is in desired state.




## Examples

To simply install Postgresql server:

```YAML
- name: Install Postgresql
  hosts: sandbox

  pre_tasks:
    - name: Update apt
      become: yes
      apt:
        cache_valid_time: 1800
        update_cache: yes
      tags:
        - build

  roles:
    - role: grim-fendango.postgresql
```

Install with custom root password

```YAML
- name: Install Postgresql
  hosts: sandbox

  vars:
    my_service:
      db:
        root_user: toor
        root_password: Pa55word:)

  pre_tasks:
    - name: Update apt
      become: yes
      apt:
        cache_valid_time: 1800
        update_cache: yes
      tags:
        - build

  roles:
    - role: grim-fendango.postgresql
      postgresql:
        root_password: "{{ my_service.db.password }}"
        root_user: "{{ my_service.db.user }}"
```

Configure db server accessibility from host machine

```YAML
- name: Install Postgresql
  hosts: sandbox

  pre_tasks:
    - name: Update apt
      become: yes
      apt:
        cache_valid_time: 1800
        update_cache: yes
      tags:
        - build

  roles:
    - role: grim-fendango.postgresql
      postgresql:
        expose_to_host: true
```
