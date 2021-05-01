Role Name
=========

This role will setup a containerd based kubernetes cluster with all nodes from a give

Requirements
------------

Ubuntu version 16+ is required.

Role Variables
--------------

`kube_version`:
description: This will set the kubernetes version to be installed
default: 1.20.1


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      gather_facts: yes
      become: yes
      roles: 
            - k8s_builder
-------

BSD

Author Information  Rockfeller Fixe
------------------
