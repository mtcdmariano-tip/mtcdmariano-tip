---
- name: Download and Extract Nagios Core
  unarchive:
    src: https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.5.tar.gz
    dest: /tmp
    copy: no

- name: Compile Nagios make clean
  make:
    chdir: /tmp/nagios-4.4.5
    target: clean

- name: Compile Nagios make all
  make:
    target: all
    chdir: /tmp/nagios-4.4.5

- name: Compile Nagios make install
  make:
    target: install
    chdir: /tmp/nagios-4.4.5

- name: Compole Nagios make install-init
  make:
    target: install-init
    chdir: /tmp/nagios-4.4.5

- name: Compile Nagios make install-config
  make:
    target: insta;;-config
    chdir: /tmp/nagios-4.4.5

- name: Compile Nagios make install-commandmode
  make:
    target: install-commandmode
    chdir: /tmp/nagios-4.4.5

- name: Fire up the web interface installer
  make:
    target: install-webconf
    chdir: /tmp/nagios-4.4.5

- name: add user account
  htpasswd:
    path: /usr/local/nagios/etc/htcpasswd.users
    name: user
    password: password

- name: a2enmod cgi
  command: a2enmod cgi

- name: restart apache2
  service:
    name: apache2
    state: restarted
