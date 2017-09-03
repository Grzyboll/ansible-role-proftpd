Role Name
=========
This role installs a Proftpd server with virtual users from a MySQL/MariaDB database instead of real system users (included Quota).
Please keep in mind that the role is dedicated to users who should have access e.g. to a web server (apache or nginx: /var/www/html path).
Mostly based on: https://www.howtoforge.com/proftpd_mysql_virtual_hosting_debian_etch

Requirements
------------
Centos/RHEL
MySQL or MariaDB (I assume that data base exists), if not check or install MySQL locally using /tests/requirements.yml.
DB can be also remotely, only edit defaults/main.yml

Role Variables
--------------
Available variables are listed below, along with default values (see defaults/main.yml):

defaults/main.yml

```
# defaults file for ansible-role-proftpd
proftpd_hostname: "{{ ansible_hostname }}"
proftpd_fqdn: "{{ ansible_fqdn }}"
proftpd_ip: "{{ ansible_default_ipv4.address }}"

# proftpd config & credentials <--- Needed to add user to SQL and define which UID and GID should be default. This defaults should have rights to /var/www/html (apache or nginx rights).
proftpd_db_user: proftpd
proftpd_db_password: 12345
proftpd_db_host: "localhost"
proftpd_default_uid: 48
proftpd_default_gid: 48

# proftpd for sample user <--- Create sample user to check if all rights and configurations are correct
proftpd_example_user: exampleuser
proftpd_example_password: secret
proftpd_example_uid: 48
proftpd_example_gid: 48
proftpd_example_path: "/var/www/html"
proftpd_sftp_port: 222

# sql config & credentials <--- Create ftp db for servicing quota
sql_db_user: root
sql_db_dbname: ftp
sql_db_hostname: "{{ ansible_hostname }}"
sql_db_password: root
```
proftpd.conf
```
# In proftpd.conf.j2 you should check:

Set the user and group that the server normally runs at.
User                            ftp
Group                           ftp
```

Dependencies
------------
None.


Example Playbook
----------------

```
ansible-playbook site.yml -i hosts -k

```

License
-------
MIT

Author Information
------------------
Grzyboll: pokiegogrzyba @gmail.com
