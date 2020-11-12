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

`nfs_export` is the path to exported filesystem mountpoint on the NFS server.

`nfs_client_mnt_point` is the path to the mountpoint on the NFS clients.

`nfs_client_mnt_options` allows passing mount options to the NFS client.

`nfs_client_mnt_state` desired state for the mount. As passed to the ansible `mount` builtin module. Can be one of: `absent`, `mounted`,
`present`, `unmounted` or `remounted`. Defaults to `mounted`.

`nfs_server` is the IP address or hostname of the NFS server.

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
          nfs_server: "{{ hostvars['nfs_server']['ansible_host'] }}"


Author Information
------------------

- Holly Silk (<holly@stackhpc.com>)
