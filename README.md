NFS Provisioner Role
=========

The nfs-provisioner role will configure a [Qubinode](https://qubinode.io/) device to use NFS for OpenShift persistant storage. It configures the [Kubernetes NFS-Client Provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner.git) and setup up the OpenShift internal registry to use a NFS backed PVC.

The nfs-provisioner configure the Qubinode as NFS server.


Requirements
------------
This role is intended to be executed on a Qubinode.


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
use_token | Using login token to connect to OpenShift Cluster. | true
provision_nfs_server  | Install and configure nfs server packages  | true
nfs_server_directory_path  |  Set Directory path of nfs storage  | /exports
provision_nfs_client_provisoner |Configure the nfs-provisioner container on OpenShift | true
configure_registry  |  Configure Registry with nfs-provisioner storage  |  false
nfs_server_ip | Set the ip address of the nfs server | 192.168.1.2
registry_pvc_size | Configure the default size of regisitry | 100Gi  
kubeconfig_dir location of auth/kubeconfig | "/home/qubi/qubinode-installer/ocp4"
nfs_project_namespace | OpenShift Project name for the nfs-provisioner | nfs-provisioner
nfs_provisioner_rbac_object_file  | default path of yaml file  | "/usr/local/src/nfs-provisioner-rbac.yaml"
nfs_provisioner_deploy_loc  | default path of yaml file  | "/usr/local/src/nfs-provisioner-deployment.yaml"
nfs_provisioner_sc_object_file  | default path of yaml file  | "/usr/local/src/nfs-provisioner-sc.yaml"
storage_class_name  |  default storage class name  |  nfs-storage-provisioner
nfs_registry_pvc_object_file  |  default path of yaml file  |   "/usr/local/src/registry-pvc.yaml"
delete_deployment  | delete the deployment and project for nfs-provisioner  | false
insecure_skip_tls_verify  |  Skip insecure tls verify  |  true
nfs_client_image| [Release page](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner/releases) | v4.0.2
Dependencies
------------

* Ansible
* OpenShift cli

Example Playbook
----------------
Example using token to deploy to nfs-provisioner OpenShift
```
- hosts: localhost
  become: yes
  vars:
    use_token: true
    provision_nfs_server: true
    nfs_server_directory_path: /export
    provision_nfs_provisoner: true
    configure_registry: true
    nfs_server_ip:  changeme
    registry_pvc_size: 100Gi
    storage_class_result: true
    openshift_token: 1234567890
    openshift_url: https://master.example.com:6443 #https://master.example.com for openshift 3
    openshift_version: ocp4
    project_namespace: nfs-provisioner
    set_as_default: true
    delete_deployment: false
    insecure_skip_tls_verify: true
  roles:
  - nfs-provisioner-role
```

Example using kubeconfig to deploy to nfs-provisioner OpenShift
```
    - hosts: targetserver
      become: yes
      vars:
        provision_nfs_server: true
        nfs_server_directory_path: /export
        provision_nfs_client_provisoner: true
        configure_registry: false
        nfs_server_ip:  changeme
        registry_pvc_size: 100Gi
        storage_class_result: true
        kubeconfig_dir: "/home/qubi/qubinode-installer/ocp4"
        openshift_version: ocp4
        nfs_project_namespace: nfs-provisioner
        delete_deployment: false
        insecure_skip_tls_verify: true
      roles:
      - nfs-provisioner-role
```

License
-------

GPLv3

Author Information
------------------

This role was created in 2019 by Tosin Akinosho
