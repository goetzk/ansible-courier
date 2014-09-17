Courier
=======

This role manages the install of Courier IMAP and performs basic configuration.
It is still at an early stage of development and should gain features over
time.

Requirements
------------

N/A

Role Variables
--------------

The only variable provided by this role controls if courier-imap-ssl will be enabled
  courier_imap_ssl_enable: true

To control the creation of the private key any variable provided by
franklinkim.openssl can be used. Eg:
openssl_certs_path: /etc/courier
openssl_keys_path: /etc/courier
openssl_self_signed: 
  - { name: 'example.com', country: 'AU', state: 'Your state', city: 'Some city', email: 'contact@example.com', days: 1095 }

Dependencies
------------

franklinkim.openssl provides the SSL certificate generation.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: goetzk.courier , courier_imap_ssl: true }

License
-------

GPL+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.

