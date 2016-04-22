Developer Mountain Environment Variables
========================================

This role will create a files in `/etc/default/` with you apps and any shared environment variables.
These files are then sourced in `.profile`, `.bashrc` & `.bash_profile`.


In you `var_file` you can have variable called `dvmtn_shared_env`, with key value pairs of environment variables.
This will create a file called `/etc/default/dvmtn_shared` with the environment variables in.

You can then define a key in `dvmtn_app_env` for every file you want in `/etc/default/`, each with key value pairs of environment variables.
These files will then source `/etc/default/dvmtn_shared`.

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
This will create file at `/etc/default/dvmtn_shared`, `/etc/default/app_1` & `/etc/default/app_2`

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
