NFS Provisioner Role
=========

nfs-provisioner used for allowing stroageclass testing in an OpenShift Platform. This role will provision and NFS server and create an nfs-provisioner class on an OpenShift Cluster.

Requirements
------------

* RHEL7 Should be installed on machine.
* Ansible should be installed on machine

Understanding OpenShift Persistent Storage Types
------------------------------------------------
Access Mode  |  CLI Abbreviation  | Description
--|---|--
ReadWriteOnce  |  RWO  | The volume can be mounted as read-write by a single node.
ReadOnlyMany  | ROX  | The volume can be mounted as read-only by many nodes.
ReadWriteMany  | RWX  | The volume can be mounted as read-write by many nodes.

Supported Access modes For PVs
-------------------------------
Volume Plug-in  | ReadWriteOnce  |  ReadOnlyMany | ReadWriteMany  targetserver
--|---|---|--
AWS EBS  | :heavy_check_mark:  | :x:  | :x:  
Azure File  | :heavy_check_mark:  | :heavy_check_mark:  | :heavy_check_mark:
Azure Disk  |  :heavy_check_mark: | :x:  | :x:
Cinder  | :heavy_check_mark:  | :x:  | :x:
Fibre Channel  | :heavy_check_mark:  | :heavy_check_mark:  | :x:
GCE Persistent Disk  | :heavy_check_mark:  | :x:  |  :x:
HostPath  | :heavy_check_mark:  |  :x: |  :x:
iSCSI  | :heavy_check_mark:  | :heavy_check_mark:  | :x:
Local volume  |  :heavy_check_mark: | :x:  | :x:
NFS  | :heavy_check_mark:  | :heavy_check_mark:  | :heavy_check_mark:
VMware vSphere  | :heavy_check_mark:  | :x:  | :x:

Role Variables
--------------
Type  | Description  | Default Value
--|---|--
provision_nfs_server  | Install and configure nfs server packages  | true
nfs_server_directory_path  |  Set Directory path of nfs storage  | /exports
provision_nfs_provisoner |Configure the nfs-provisioner container on OpenShift | true
nfs_server_ip | Set the ip address of the nfs server | 192.168.1.2
registry_pvc_size | Configure the default size of regisitry | 60Gi

Dependencies
------------

* Ansible
* OpenShift cli

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: targetserver
      roles:
         - nfs-provisioner-role

License
-------

GPLv3

Author Information
------------------

This role was created in 2019 by Tosin Akinosho
