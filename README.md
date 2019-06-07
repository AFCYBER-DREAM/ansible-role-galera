ansible-role-galera
=========
[![Build Status](https://travis-ci.com/AFCYBER-DREAM/ansible-role-galera.svg?branch=master)](https://travis-ci.com/AFCYBER-DREAM/ansible-role-galera)

A secure galera deployment

Requirements
------------

This role requires the use of the ansible-role-repositories and the ansible-role-packages role. The required repos and packages are defined
 in this roles defaults.


Role Variables
--------------
### List of configurable variables

##### Custer Options
- galera_cluster_name
  - Name of the galera cluster the (default is galera-cluster)

##### Networking Options 
- galera_cluster_bind_interface
  - Interface to bind mysql
  - Normally this is a private network interface
- galera_cluster_bind_address 
  - Defaults to ip address of bound interface

##### Replications Options
- galera_wsrep_node_address
  - Address of the replication interface
  - This will default to the galera_cluster_bind_interface and galera_wsrep_node_address_port 
- galera_wsrep_node_address_port: 
  - Port to bind for replication

##### Security Options
There are a few different options when configuring the security settings for the role. Connection encryption is on of those options.

- galera_mysqld_ssl
  - Turn on mysqld ssl for connections

- galera_ssl
  - Turn on Galera Replications Encryption

The certificates that are going to be used need to defined as a variable in the host or group vars file in the following format. They can be vaulted for security

Group Variables:
```
galera_certs:
    ca_cert: |
        -----BEGIN CERTIFICATE-----
        MIIEvQIBADANBgkqhki...
        -----END CERTIFICATE-----
```

Host Variables:
```
galera_host_certs:
    key: |
        -----BEGIN PRIVATE KEY-----
        MIIEvQIBADANBgkqhki...
        -----END PRIVATE KEY-----
    cert: |
        -----BEGIN CERTIFICATE-----
        MIIEvQIBADANBgkqhki...
        -----END CERTIFICATE-----
```

The location of where the certificates are installed can be also defined:

SELinux variables: 

SELinux configuration is the standard option if you are deploying on RHEL/CentOS based systems. All of the options are defined in the defaults along with a TE files that is require for proper usage. The ports defined here are also used to configure the firewall. 

```
galera_security:
  selinux_te_files:
    - { name: "afcyber-galera", version: "1.0", te_file: "afcyber-galera.te"}
  security_ports:
    - { port: 3306, protocol: 'tcp', port_type: 'mysqld_port_t'}
    - { port: 4444, protocol: 'tcp', port_type: 'mysqld_port_t'}
    - { port: 4567, protocol: 'tcp', port_type: 'mysqld_port_t'}
    - { port: 4567, protocol: 'udp', port_type: 'mysqld_port_t'}
    - { port: 4568, protocol: 'tcp', port_type: 'mysqld_port_t'}

```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: ansible-role-galera }

License
-------

MIT

Author Information
------------------
The Development Range Engineering, Architecture, and Modernization (DREAM) Team
