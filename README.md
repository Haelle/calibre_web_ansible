# Role Name

This Ansible role install [Calibre Web](https://github.com/janeczku/calibre-web) with Nginx and Pip

_Warning_, this role runs as _sudo_.

## Warnings

There are 4 manuals steps to do after an installation:

1. Go to your_server:8085 to setup your library. You'll have to do the other steps

2. Set the Calibre directory.
There is no way to set the Calibre directory where your books are from the command line.
You need to set it manually from the web interface.

3. Set the Calibre-Web port
This is the same for the port, if different from 8085, you need to change it in the web interface.

4. Update the admin password
Default admin login:
Username: admin
Password: admin123

You should change them also within the interface

## Role Variables

### Mandatory variables

1. `calibre_web.fqdn`: fully qualified domain name on which create the Nginx conf
1. `certbot_email`: the email to register the domain name certificate for Certbot

### Optional variables

1. `calibre_web.name`: the name of the calibre library (usefull if you install many libraries)
2. `calibre_web.db_symlink_path`: path to your `app.db` of Calibre-Web (usefull if you want to sync it in the cloud, if not defined it will create a new one)
3. `calibre_web.port`: internal port for nginx proxy pass, when having many Calibre-web they should be differents ports (default is 8085).

### Warnings about port

The service can return this error: `Error starting server: Address already in use`. It means that the port is already in use. As the default port is 8085 if you have another Calibre running on 8085 you cannot even start the new one without stoping the previous one.

So you should also avoid using the default port 8085 so that new servers can at least start.

## Requirements

Install requirements (Certbot):
```
ansible-galaxy install -r requirements.yml
```

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
        fqdn: library.example.com
        certbot_email: admin@example.com
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
        fdqn: library.example.com
        db_symlink_path: /path/to/calibre_web/app.db'
        certbot_email: admin@example.com
```

## License

BSD
