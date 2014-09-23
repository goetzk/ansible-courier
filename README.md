Courier Mail Server
===================

This role manages the install and configuration of the courier mail system
including IMAP, IMAP-SSL, MTA, MTA-SSL, MSA and Courier Authentication Daemon.
The templates are based on Debians packaging of version 4.10 but may work with
other versions as well.

Other parts of the Courier Mail Server could be added relatively easily as
could support for non Debian platforms.

Requirements
------------

N/A

Role Variables
--------------

```
# Imapd configuration, see templates/imapd.tmpl for details of the options
# Default address for ports which don't specify (0 means any)
courier_imap_address: '0'
# Default port 
courier_imap_port: '143'
# Location of pid file
courier_imap_pidfile: '/var/run/courier/imapd.pid'
# Should the server check for mail in every folder
courier_imap_check_all_folders: '0'
# Mask of files created by imap server
courier_imap_umask: '022'
# Name of the Trash folder, useful for Outlook Express compatibility
courier_imap_trashfoldername: 'Trash'
# Adding comma separated Folder:value pairs will turn on purging of the Folder
# contents when it reaches value days old.
courier_imap_emptytrash: ''
# Should Courier IMAP be started?
courier_imap_start_daemon: 'yes'
# Maildir name
courier_imap_maildir: 'Maildir'

# Should we install/enable openssl support?
courier_imap_ssl_enable: true

# Imapd configuration, see templates/imapd.tmpl for details of the options
# Default address for ports which don't specify (0 means any)
courier_imap_ssl_address: '0'
# Default port 
courier_imap_ssl_port: '993'
# Location of pid file
courier_imap_ssl_pidfile: '/var/run/courier/imapd-ssl.pid'
# Should we disable SSL on port 993 and just use TLS on 143
courier_imap_ssl_start: 'yes'
# Should all connections be over TLS
courier_imap_tls_required: 0
# certificate file for secure connections
courier_imap_tls_certfile: /etc/courier/imapd.pem
# Location of CAs (trusted certificates)
courier_imap_tls_trust_certs: /etc/ssl/certs
# What form of client certificate verification to use
courier_imap_tls_verifypeer: 'NONE'
# Maildir name
courier_imap_ssl_maildir: 'Maildir'

# Authentication backend to use, see templates/authdaemonrc for the full list.
courier_authdaemon_authmodulelist: 'authpam'
# Log debug information to syslog, 0 disables, 2 saves passwords in plain text
courier_authdaemon_debug_login: '0'
# Extra application options
courier_authdaemon_defaultoptions: ''
# Logging options
courier_authdaemon_loggeropts: ''


To control the creation of the private key any variable provided by
franklinkim.openssl can be used. See Example Playbook below.
# Where should the key/cert go?
openssl_certs_path: /etc/courier
openssl_keys_path: /etc/courier

Checked in imapd configuration template to see if / is listed, if it is an
alternative path is used for the shared index. This should not need to be set
manually and comes in from goetzk.filesystems.
filesystems_configuration_ro: []
```

Dependencies
------------

franklinkim.openssl provides the SSL certificate generation.
goetzk.filesystems includes filesystems_configuration_ro which is used in the template configuration. To avoid this dependency it should be possible to set filesystems_configuration_ro.name to ''.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      vars:
        - openssl_certs_path: /etc/courier
        - openssl_keys_path: /etc/courier
        - openssl_self_signed: 
          - { name: 'example.com', country: 'AU', state: 'Your state', city: 'Some city', email: 'contact@example.com', days: 1095 }
      roles:
        - { role: goetzk.courier , courier_imap_ssl: true }

License
-------

GPL+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.

