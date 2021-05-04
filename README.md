Kubernetes Cluster
==================

The vagrant file will spun up one Control Plane and three Worker nodes. Since Docker is deprecated starting Kubernetes v1.20, Containerd is used as the container runtime. Kubernetes was indirectly using containerd through docker. More info can be found on link below.  
[K8s docker Deprecation](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/)


Control Plane:
3GB memory,
2 CPU

Worker Nodes:
2GB memory,
2 CPU

Requirements
============
Ansible==2.9  
Python3.5+  
VirtualBox  
vagrant 2.1+  
Linux 16GB+ memory  

How to Use
==========
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
```
vagrant status
Current machine states:

cp1                       running (virtualbox)
node1                     running (virtualbox)
node2                     running (virtualbox)
node3                     running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```
ssh to the any box in the cluste by using the node name  
`vagrant ssh <node_name>`  
for example  
`vagrant ssh cp1`  

Cluster Validation
==================
```
kubectl get nodes -o wide
NAME       STATUS   ROLES                  AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
c1-cp1     Ready    control-plane,master   13m   v1.20.1   192.168.58.100   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
c1-node1   Ready    <none>                 13m   v1.20.1   192.168.58.101   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
c1-node2   Ready    <none>                 13m   v1.20.1   192.168.58.102   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
c1-node3   Ready    <none>                 13m   v1.20.1   192.168.58.103   <none>        Ubuntu 18.04.5 LTS   4.15.0-128-generic   containerd://1.3.3
```

```
kubectl get pods --all-namespaces -o wide
NAMESPACE     NAME                                       READY   STATUS    RESTARTS   AGE   IP               NODE       NOMINATED NODE   READINESS GATES
kube-system   calico-kube-controllers-6d7b4db76c-f59ql   1/1     Running   0          54m   10.244.13.194    c1-cp1     <none>           <none>
kube-system   calico-node-78zm9                          1/1     Running   0          54m   192.168.58.103   c1-node3   <none>           <none>
kube-system   calico-node-n5596                          1/1     Running   0          54m   192.168.58.102   c1-node2   <none>           <none>
kube-system   calico-node-sz7fk                          1/1     Running   0          54m   192.168.58.101   c1-node1   <none>           <none>
kube-system   calico-node-zjvsf                          1/1     Running   0          54m   192.168.58.100   c1-cp1     <none>           <none>
kube-system   coredns-74ff55c5b-cksnk                    1/1     Running   0          54m   10.244.13.193    c1-cp1     <none>           <none>
kube-system   coredns-74ff55c5b-jfd5m                    1/1     Running   0          54m   10.244.13.195    c1-cp1     <none>           <none>
kube-system   etcd-c1-cp1                                1/1     Running   0          54m   192.168.58.100   c1-cp1     <none>           <none>
kube-system   kube-apiserver-c1-cp1                      1/1     Running   0          54m   192.168.58.100   c1-cp1     <none>           <none>
kube-system   kube-controller-manager-c1-cp1             1/1     Running   0          54m   192.168.58.100   c1-cp1     <none>           <none>
kube-system   kube-proxy-2f6k5                           1/1     Running   0          54m   192.168.58.101   c1-node1   <none>           <none>
kube-system   kube-proxy-5m5ds                           1/1     Running   0          54m   192.168.58.103   c1-node3   <none>           <none>
kube-system   kube-proxy-6l8sv                           1/1     Running   0          54m   192.168.58.100   c1-cp1     <none>           <none>
kube-system   kube-proxy-hnb8q                           1/1     Running   0          54m   192.168.58.102   c1-node2   <none>           <none>
kube-system   kube-scheduler-c1-cp1                      1/1     Running   0          54m   192.168.58.100   c1-cp1     <none>           <none>
```

```
kubectl get all --all-namespaces
NAMESPACE     NAME                                           READY   STATUS    RESTARTS   AGE
kube-system   pod/calico-kube-controllers-6d7b4db76c-f59ql   1/1     Running   0          56m
kube-system   pod/calico-node-78zm9                          1/1     Running   0          55m
kube-system   pod/calico-node-n5596                          1/1     Running   0          55m
kube-system   pod/calico-node-sz7fk                          1/1     Running   0          55m
kube-system   pod/calico-node-zjvsf                          1/1     Running   0          56m
kube-system   pod/coredns-74ff55c5b-cksnk                    1/1     Running   0          56m
kube-system   pod/coredns-74ff55c5b-jfd5m                    1/1     Running   0          56m
kube-system   pod/etcd-c1-cp1                                1/1     Running   0          56m
kube-system   pod/kube-apiserver-c1-cp1                      1/1     Running   0          56m
kube-system   pod/kube-controller-manager-c1-cp1             1/1     Running   0          56m
kube-system   pod/kube-proxy-2f6k5                           1/1     Running   0          55m
kube-system   pod/kube-proxy-5m5ds                           1/1     Running   0          55m
kube-system   pod/kube-proxy-6l8sv                           1/1     Running   0          56m
kube-system   pod/kube-proxy-hnb8q                           1/1     Running   0          55m
kube-system   pod/kube-scheduler-c1-cp1                      1/1     Running   0          56m
NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  56m
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   56m
NAMESPACE     NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   daemonset.apps/calico-node   4         4         4       4            4           kubernetes.io/os=linux   56m
kube-system   daemonset.apps/kube-proxy    4         4         4       4            4           kubernetes.io/os=linux   56m
NAMESPACE     NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/calico-kube-controllers   1/1     1            1           56m
kube-system   deployment.apps/coredns                   2/2     2            2           56m
NAMESPACE     NAME                                                 DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/calico-kube-controllers-6d7b4db76c   1         1         1       56m
kube-system   replicaset.apps/coredns-74ff55c5b                    2         2         2       56m
```

Learn by doing
==============
One may learn by using this tutorial  
https://kubernetesbyexample.com/


Teardown Cluster
================
Simply run below command to removes all vagrant nodes.  
`vagrant destroy -f`
