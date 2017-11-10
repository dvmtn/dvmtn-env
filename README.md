Developer Mountain Environment Variables
========================================

An Ansible role for creating and sourcing environment variable scripts in `/etc/default`.
These scripts are will be sourced in `.profile`, `.bashrc` & `.bash_profile` to ensure you envvars are available to apps, users and cronjobs.

There are 2 types of files created:

  1. App specific files. These are sourced for one particular apps environment.
  2. An optional, single shared script called `dvmtn_shared`. This is sourced by all app scripts. Use it to keep shared config in one place, such as monitoring tool API tokens.


Installation
------------

`$ ansible-galaxy install dvmtn.env`


Setup
-----

  1. Declare a dictionary called `dvmtn_app_env`.
  2. Create a key under `dvmtn_app_env` for each app.
  3. Create a dictionary under each app. Keys will become the name of an environment variable, with the values becoming the value of the environment variable.
  4. (Optional) Declare a dictionary called `dvmtn_shared_env`. The keys will become environment variables with the value given, inside `/etc/default/dvmtn_shared`.

Example Playbook
----------------

    - hosts:
        - my_app
      vars_files:
        - "host_vars/{{ inventory_hostname }}_vault.yml"
      roles:
      - role: dvmtn.env


Example multi-app usage
-----------------------

    dvmtn_shared_env:
      RAILS_ENV: "qa"
      SECRET_KEY_BASE: "1234"
      DATABASE_HOST: localhost
      DATABASE_PORT: 5432

    dvmtn_app_env:
      app_1:
        DATABASE_DATABASE: "{{ app_1_database }}"
        DATABASE_USERNAME: "{{ app_1_username }}"
        DATABASE_PASSWORD: "{{ app_1_password }}"
      app_2:
        DATABASE_DATABASE: "{{ app_2_database }}"
        DATABASE_USERNAME: "{{ app_2_username }}"
        DATABASE_PASSWORD: "{{ app_2_password }}"

This will create 3 files: `/etc/default/dvmtn_shared`, `/etc/default/app_1` & `/etc/default/app_2`

Requirements
------------

Tested on ubuntu 14.04 and 16.04 LTS but it should work on other versions that are similar.

Ansible galaxy
--------------

You can find it on the official ansible galaxy [here](https://galaxy.ansible.com/dvmtn/env/).

License
-------

MIT
