# Role Name

This Ansible role install [Calibre Web](https://github.com/janeczku/calibre-web) with Nginx and Pip

_Warning_, this role runs as _sudo_.

## Warnings

There are 3 manuals steps to do after an installation:

1. Set the Calibre directory.
There is no way to set the Calibre directory  where your books are from the command line.
You need to set it manually from the web interface.

2. Set the Calibre-Web port
This is the same forthe port, if different from 8085, you need to change it in the web interface.

3. Update the admin password
Default admin login:
Username: admin
Password: admin123

You should change them also within the interface

## Role Variables

### Mandatory variables

1. `calibre_web.domain`: domain on which create the Nginx conf (URL would be
   _default_library.example.com_

### Optional variables

1. `calibre_web.name`: the name of the calibre library (usefull if you install many libraries)
2. `calibre_web.db_symlink_path`: path to your `app.db` of Calibre-Web (usefull if you want to sync it in the cloud, if not defined it will create a new one)
3. `calibre_web.port`: internal port for nginx proxy pass, when having many Calibre-web they should be differents ports (default is 8085)

## Dependencies

This role has not dependencies but will install :
- git
- nginx
- pip

## Example Playbook

Minimal playbook:
```yaml
---
- hosts: servers
  become: 'yes'

  roles:
    - role: calibre_web_ansible
      calibre_web:
        domain: example.com
```

Full playbook :
```yaml
---
- hosts: servers
  become: 'yes'

  roles:
    - role: calibre_web_ansible
      calibre_web:
        name: default_library
        port: 8085
        domain: example.com
        db_symlink_path: /path/to/calibre_web/app.db'
```

## License

BSD
