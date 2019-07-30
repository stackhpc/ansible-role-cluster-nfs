Cluster NFS
===============

A simple NFS server and client playbook intended for Cluster-as-a-Service cloud
deployments.

Requirements
------------

None

Role Variables
--------------

`nfs_fstype` is the type of filesystem to create on the disk.

`nfs_disk_location` is the target path to the disk.

`nfs_server_hostname` is the hostname or IP of the nfs server 

`nfs_export` is the path to exported filesystem mountpoint on the nfs server.

`nfs_client_mnt_point` is the path to the mountpoint on the nfs clients.

Groups
------

The groups `nfs_server` and `nfs_clients` must exist.

Dependencies
------------

None

Example Playbook
----------------

    ---
    - hosts:
      - nfs_server
      - nfs_clients
      become: yes
      roles:
        - role: stackhpc.nfs-cluster
          nfs_enable:
            server: "{{ inventory_hostname in groups['nfs_server'] }}"
            clients: "{{ inventory_hostname in groups['nfs_clients'] }}"

Author Information
------------------

- Holly Silk (<holly@stackhpc.com>)
