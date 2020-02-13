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

`nfs_server` is the name or address of the nfs server.

`nfs_export` is the path of the exported filesystem mountpoint on the nfs server.

`nfs_client_mnt_point` is the path to the mountpoint on the nfs clients.

`nfs_client_mnt_options` allows passing mount options to the NFS client.

If filesystem creation is not required then `nfs_fstype` and `nfs_disk_location` can be omitted.

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
        - role: stackhpc.nfs
          nfs_enable:
            server: "{{ inventory_hostname in groups['nfs_server'] }}"
            clients: "{{ inventory_hostname in groups['nfs_clients'] }}"
          nfs_server: "{{ groups['nfs_server'][0] }}"
          nfs_export: "{{ hostvars['nfs_server']['nfs_export'] }}"


Author Information
------------------

- Holly Silk (<holly@stackhpc.com>)
