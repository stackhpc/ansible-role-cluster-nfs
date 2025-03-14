Cluster NFS
===============

A simple NFS server and client playbook intended for Cluster-as-a-Service cloud
deployments.

Requirements
------------

None

Role Variables
--------------

`nfs_fstype` is the type of filesystem to create on the disk. Optional, default "xfs".

`nfs_disk_location` is the path to the block device on which to create a filesystem for export. Optional, default does not create a filesystem (e.g. as when exporting an existing directory).

`nfs_export` is the path to exported filesystem mountpoint on the NFS server. Optional, default "/srv".

`nfs_export_clients` is the client list allowed to mount the filesystem.
See "Machine Name Formats` in `man exports`. Optional string, default "*".
Note that multiple clients may be specified as a space-separated string (not a yaml list).

`nfs_export_options` are the options to apply to the export. Optional, default "rw,secure,root_squash".

`nfs_client_mnt_point` is the path to the mountpoint on the NFS clients. Optional, default "/mnt".

`nfs_client_mnt_options` allows passing mount options to the NFS client. Optional, default "defaults,nosuid,nodev".

`nfs_client_mnt_state` desired state for the mount. As passed to the ansible `mount` 
builtin module. Can be one of "absent", "mounted", "present", "unmounted" or 
"remounted". Optional, default "mounted".

`nfs_server` is the IP address or hostname of the NFS server.

`nfs_enable`: a mapping with keys `server` and `client` - values are bools determining the role of the host.

Multiple NFS client/server configurations may be provided by defining `nfs_configurations`. This should be a list of mappings with keys/values are as per the variables above. Omitted keys/values are filled from the corresponding variable. For example if all configurations require the same non-default client mount options, define `nfs_client_mnt_options` and omit the key "nfs_client_mnt_options" from all configuration mappings.

Dependencies
------------

None

Example Playbook
----------------

Assuming:
- An inventory group `nfs_server` containing a single host
- An inventory group `nfs_clients` containing one or more clients
- The hostvar `ansible_host` containing hosts' IP address

the example below configures a root-squashed read/write share which can only
be mounted by the clients.

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
          nfs_export_clients: "{{ groups['nfs_clients'] | map('extract', hostvars, 'ansible_host') | join(' ') }}"

Author Information
------------------

- Holly Silk (<holly@stackhpc.com>)
