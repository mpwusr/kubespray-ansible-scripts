## kubespray-ansible-scripts
Ansible scripts to deploy multi-master HA kubernetes cluster with kubespray kubeadm

# Quickstart documentation

> Download kubernetes installer from code repository
first, change directory to working dir for example (~/working) and clone the github repository:

```
$ cd ~/working

$ git clone https://github.com/mpw07458/kubespray-ansible-scripts.git

$ cd kubespray-ansible-scripts

```

> Creating infrastructure for China AWS

```

$ cd contrib/terraform

$ cd aws-cn

```
> Make sure environment file "cn-northwest-1.env" is correct

```
$ cat cn-northwest-1.env
#AWS Access Key
AWS_ACCESS_KEY_ID = ""
#AWS Secret Key
AWS_SECRET_ACCESS_KEY = ""
#EC2 SSH Key Name
AWS_SSH_KEY_NAME = ""
#AWS Region
AWS_DEFAULT_REGION = "cn-northwest-1"
```
> make sure AWS environment var are correct

> Run ansible playbook

```
$ chmod +x create_cn_infra.sh

$ ./create_cn_infra.sh

```

> Creating infrastructure for US AWS

```

$ cd contrib/terraform

$ cd aws-us

```
> Make sure environment file "us-west-1.env" is correct

```
$ cat us-west-1.env
#AWS Access Key
AWS_ACCESS_KEY_ID = ""
#AWS Secret Key
AWS_SECRET_ACCESS_KEY = ""
#EC2 SSH Key Name
AWS_SSH_KEY_NAME = ""
#AWS Region
AWS_DEFAULT_REGION = "us-west-1"
```

> run ansible playbook
```
$ chmod +x create-us-infra.sh

$ ./create-us-infra.sh
```

> Save state object for infrastructure

```
$ mkdir {$Date}-state

$ cp terraform.state  {$Date}-state/.
```
> Make sure the inventory/hosts file is correct for the infrastructure

```
$ cat inventory/hosts
[all]
kubernetes-test-master0 ansible_host=10.250.207.40
kubernetes-test-master1 ansible_host=10.250.221.110
kubernetes-test-master2 ansible_host=10.250.193.183
kubernetes-test-worker0 ansible_host=10.250.204.107
kubernetes-test-worker1 ansible_host=10.250.217.217
kubernetes-test-worker2 ansible_host=10.250.200.7
kubernetes-test-etcd0 ansible_host=10.250.197.25
kubernetes-test-etcd1 ansible_host=10.250.215.198
kubernetes-test-etcd2 ansible_host=10.250.195.205
bastion ansible_host=18.144.29.210
bastion ansible_host=13.56.179.128

[bastion]
bastion ansible_host=18.144.29.210
bastion ansible_host=13.56.179.128

[kube-master]
kubernetes-test-master0
kubernetes-test-master1
kubernetes-test-master2


[kube-node]
kubernetes-test-worker0
kubernetes-test-worker1
kubernetes-test-worker2


[etcd]
kubernetes-test-etcd0
kubernetes-test-etcd1
kubernetes-test-etcd2


[k8s-cluster:children]
kube-node
kube-master


[k8s-cluster:vars]
apiserver_loadbalancer_domain_name="kubernetes-elb-test-53736937.us-west-1.elb.amazonaws.com"
```
