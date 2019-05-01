role-urbackup
========= 

UrBackup is an easy to setup Open Source client/server backup system, that through a combination of image and file backups accomplishes both data safety and a fast restoration time.

This role is designed for deploying an open source disk-2-disk back-up solution in an enterprise environment and contains the server and the client installation. 

This role contains:
* Urbackup server
* Urbackup client for Ubuntu (14.04, 16.04, 18.04)
* Urbackup Client for Windows

Requirements
------------

* Ubuntu server 18.04
* Ansible control server

* administrator account with sudo rights needed.


Role Variables
--------------

To many options to describe here, please look at
the " default/main.yml" for all options and documentation.

Dependencies
------------

For using https on the server, you can use an selfsigned certificate by using the role app-cert-selfsigned.

* https://github.com/bitfinity-nl/app-cert-selfsigned


Example Playbook: Server at the bare minimum with client.
---------------------------------------------------------

    - hosts: backup-srv01
      become: true

      vars: 
        # -- custom settings: utl-urbackup --
        ubk_ubt_srv_state           : 'enabled'
        ubk_ubt_cli_state           : 'enabled'

       roles:
        - role-urbackup         

Example Playbook: Server at the bare minimum with client.
---------------------------------------------------------
    - hosts: ubuntu-client01
      become: true

      vars:
        # -- custom settings: role-urbackup --
        ubk_ubt_cli_state          : 'enabled'

      roles:
        - role-urbackup

Example Playbook: Server with selfselfsigned certificate with client. 
---------------------------------------------------------------------

    - hosts: backup-srv01
      become: true

      vars:   
        # -- custom settings: app-cert-selfsigned --
        cert_state                  : 'enabled'
        cert_base_path              : '/opt/ansible/system/ssl'
        cert_days                   : '365'
        cert_rsa                    : '4096'
        cert_keyout                 : '{{ cert_base_path }}/private/{{ ansible_hostname }}/private-selfsigned.key'
        cert_out                    : '{{ cert_base_path }}/certs/{{ ansible_hostname }}/certificate-selfsigned.crt'
        cert_country                : 'NL'
        cert_state_prov             : 'Limburg'
        cert_locality               : 'Venlo'
        cert_orgname                : 'example.com'
        cert_orgunit                : 'IT Operations'
        cert_comname                : '{{ ansible_hostname }}'
        cert_email                  : 'support@example.com'      

        # -- custom settings: utl-urbackup --
        ubk_ubt_srv_state           : 'enabled'
        ubk_ubt_srv_cert_state      : 'enabled'       
        ubk_ubt_srv_cert_file       : '{{ ap_sslcertificatefile }}'
        ubk_ubt_srv_cert_key_file   : '{{ ap_sslcertificatekeyfile }}'
        ubk_ubt_srv_cert_chain_file : '{{ ap_sslcertificatechainfile }}'
        ubk_ubt_cli_state           : 'enabled'
      
      roles:
        - app-cert-selfsigned
        - role-urbackup
         


License
-------

GPLv3

Author Information
------------------

E: lrutten@bitfinity.nl

I: https://www.bitfinity.nl
