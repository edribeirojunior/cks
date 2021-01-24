# CKS Vagrant

This repository contains a Vagrant file and scripts to create a K8s Cluster with 1 master node and 2 worker nodes (one with docker runtime and another with containerd)

The scripts are based in the https://github.com/killer-sh/cks-course-environment and https://github.com/pksheldon4/cks-cluster. 

The scripts are tested in a Windows env with Vagrant `2.2.2`. 

You can learn more about Vagrant [here](https://learn.hashicorp.com/collections/vagrant/getting-started)
You can start the cluster using `vagrant up` and refresh the cluster using `vagrant halt && vagrant destroy -f && vagrant up`
