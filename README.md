Kubernetes Cluster
==================

The vagrant file will spun up one Control Plane and three Worker nodes. Since Docker is deprecated starting Kubernetes v1.20, Containerd is used as the container runtime. Kubernetes was indirectly using containerd through docker. More info can be found on link below
https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/

Control Plane
3GB memory
2 CPU

Worker Nodes
2GB
2 CPU

Requirements 
============
Ansible==2.9
Python3.5+
vagrant 2.1+
Linux/OSX 16GB+ memory

How to Use:
===========
install virtualenv if missing
`pip3 list | grep virtualenv || pip3 install virtualenv`
create virtual environment in created dir
`mkdir vagrant_venv && python3 -m venv $(pwd)/vagrant_venv`
activate the python3 environment
`source vagrant_venv/bin/activate`
install required packages
`pip3 install -r requirements.txt`
provision the cluster, takes at least 15min
`vagrant up`
get cluster status
`vagrant status`
ssh to the any box in the cluste by using the node name
`vagrant ssh <node_name>`
`vagrant ssh cp1`

Cluster Validation
==================
```
vagrant@c1-cp1:~$ kubectl get nodes -o wide
NAME       STATUS   ROLES                  AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
c1-cp1     Ready    control-plane,master   13m   v1.20.1   192.168.58.100   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
c1-node1   Ready    <none>                 13m   v1.20.1   192.168.58.101   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
c1-node2   Ready    <none>                 13m   v1.20.1   192.168.58.102   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
c1-node3   Ready    <none>                 13m   v1.20.1   192.168.58.103   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
```



