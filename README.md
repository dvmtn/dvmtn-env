Developer Mountain Environment Variables
========================================

This role will create a file in `/etc/default/{{app_name}}` with you apps environment variables.
This file is then sourced in `.profile`, `.bashrc` & `.bash_profile`.


In you `var_file` you need a variable called `dvmtn_deploy_env`, with key value pairs of environment variables.

For boxes with more than on application you can optionally define `dvmtn_deploy_extra_env`.
This will put variables in `dvmtn_deploy_env` into `/etc/default/dvmtn_common` and create a file in `/etc/default/` for every key that sources `/etc/dvmtn_common`.

Example Playbook
----------------

    - hosts:
        - rails
      vars_files:
        - "host_vars/{{ inventory_hostname }}_vault.yml"
      roles:
      - role: dvmtn-env
      tags:
        - configure-app
        - rails
        -

Example var_file for one app deploy
-----------------------------------
This will create one file at `/etc/default/{{app_name}}`

    dvmtn_deploy_env:
      RAILS_ENV: "qa"
      SECRET_KEY_BASE: "1234"
      DATABASE_HOST: localhost
      DATABASE_PORT: 5432

Example var_file for multi app deploy
-------------------------------------
This will create file at `/etc/default/dvmtn_common`, `/etc/default/app_1` & `/etc/default/app_2`

    dvmtn_deploy_env:
      RAILS_ENV: "qa"
      SECRET_KEY_BASE: "1234"
      DATABASE_HOST: localhost
      DATABASE_PORT: 5432

    dvmtn_deploy_extra_env:
      app_1:
        DATABASE_DATABASE: "{{ app_1_database }}"
        DATABASE_USERNAME: "{{ app_1_username }}"
        DATABASE_PASSWORD: "{{ app_1_password }}"
      app_2:
        DATABASE_DATABASE: "{{ app_2_database }}"
        DATABASE_USERNAME: "{{ app_2_username }}"
        DATABASE_PASSWORD: "{{ app_2_password }}"
