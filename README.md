# k8s-sros-macvlan
some configuration files to create a kubernetes pod of SROS based on vrnetlab image 
sros image must be built in advance using the vrnetlab information
Also, you need to add license string into sros-tftpboot.yaml file before to use.
Get license via your Nokia representative

We assume you kubernetes cluster has been installed in advance
Check https://github.com/cloud-native-everything/srlinux-ansible-lab for more details
In case of using VMs for the k8s cluster, you will need to enable nested virtualization

![Intended Topology] (https://www.cloud-native-everything.com/wp-content/uploads/2021/12/sros-pod-kubernetes-topology-macvlan-passthru.png)

## Enable Nested Virtualization

If you are using Fedora, you can enable nested virtualization as follow:

```
sudo modprobe -r kvm_intel
sudo modprobe kvm_intel nested=1
```

add 'options kvm_intel nested=1' to your file '/etc/modprobe.d/kvm.conf'
You will have to reboot after that


## Create SROS pod

Create your configmap using sros-tftpboot.yaml
```
kubectl apply -f sros-tftpboot.yaml
```

Then create cni macvlan plugins
```
kubectl apply -f srospod_macvlan_cni.yaml
```

And finally, create your pod
```
srospod_macvlan.yaml
``` 

