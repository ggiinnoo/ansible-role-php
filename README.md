Setup php versions using remi
=========

This playbook allows you to install multiple php versions. With seperate confs.

Available/tested php versions are:
```
php56
php70
php71
php72
php73
php74
```

TODO
----
-

Requirements
------------

There are no requirements as of yet


Role Variables
--------------

Setting a default php version allows you to use the cli `php` command without any numbers behind it.
the var that needs to be set is: `phpVersionDefault`. Example: `phpVersionDefault: 80`

The default process manager is `ondemand` with a child size of `100`. To change this for example user `default`, set the following config in a config file:

```
Users:
  - name: "default"
    fpm:
      fpmPm: dynamic
      fpmMaxChild: 50
      fpmStartServers: 5
      fpmMinSpareServers: 5
      fpmMaxSpareServers: 35
```

Adding a php version can be done like so in a yml file
```
php56:
  packages:
    - php56
    - php56-php-cli
    - php56-php-common
    - php56-php-fpm
    - php56-php-pear
    - php56-php-pecl-jsonc
    - php56-php-pecl-zip
    - php56-php-process
    - php56-php-xml
    - php56-runtime
  fpm:
    fpmUser: apache
    fpmGroup: apache
    fpmListenOwner: apache
    fpmListenGroup: apache
    fpmListenMode: 0660
    fpmSock: /var/run/php56-fpm.sock
    fpmListenAllowedClients: 127.0.0.1
    fpmStatusPath: "/status"
    fpmSlowLog: "{{ fpmPhp56LogLocation }}/www-slow.log"
    fpmErrorLog: "{{ fpmPhp56LogLocation }}/www-error.log"
    fpmSessionSaveHandler: files
    fpmSessionsavePath: /var/lib/php/session
    fpmSessionSaveCache: /var/lib/php/wsdlcache
  default: true
```
Always make sure the correct user and groups are set for youre env.

It's possible to change the listen port of the php.conf. If you have a webserivce lisetening to an other port other then 80 change `webServicePort`

Dependencies
------------

None

Example Playbook
----------------

    - name: Setup webserver
      hosts: all
      roles:
        - ggiinnoo.php

License
-------

BSD

Author Information
------------------

    Creator: Gino Jansen
    Website: www.ginojansen.nl
