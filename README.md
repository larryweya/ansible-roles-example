# Collection of Ansible Roles

All the roles expose variables that can be overridden through host/group variables. These variables are documented on the relevant role's README.

## Usage

1. Create an [Ansible](http://www.ansible.com/get-started) project or clone this repo as a starting point. The structure looks something like this.

```
/my-ansible-project/
  /group_vars/
  /host_vars/
  /roles/ - Subtree (http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/) pull from the subprojects
  /production - Production inventory file
  /development - Development inventory file
  /playbook.yml
```

2. Create an inventory file

  `/production_inventory`
  ```
  [web1]
  192.168.33.100 ansible_ssh_user=vagrant

  [web2]
  192.168.33.101 ansible_ssh_user=vagrant

  [webservers:children]
  web1
  web2
  ```

3. Override some variables

  `/host_vars/web1`
  ```
  php_extensions:
    - php5-mcrypt
  php5_fpm_pools:
    - name: my_service
      prefix: /var
      user: vagrant
      group: vagrant
      socket_listen_owner: nginx
      socket_listen_group: nginx
      process_max_requests: 500
      access_log_path: /var/log/php5-fpm-$pool.access.log
  nginx_service_enabled: no
  ```

4. Create a playbook and add some roles

  `/my_playbook.yml`
  ```
  - hosts: webservers
    roles:
      - common
      - nginx
      - php5
      - php5-fpm
  ```

## Running
`
ansible-playbook my-playbook.yml -i inventory_file [-u vagrant] [--private-key=~/.ssh/my-private-key]
`

## Available Roles
- [common](https://github.com/larryweya/ansible-common) - plays that setup required utilities e.g. python-software-properties to provide apt-add-repository
- [nginx](https://github.com/larryweya/ansible-nginx)
- [php5](https://github.com/larryweya/ansible-php5)
- [php5-fpm](https://github.com/larryweya/ansible-php5-fpm)
- [monit](https://github.com/larryweya/monit)
