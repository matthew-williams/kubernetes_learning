#Kubernetes Learning
=========

Install and configure a basic kubernetes cluster and run the stable/minecraft helm chart on it.

- This was primarily for me to learn how to use kubernetes

##Requirements
------------

 - ansible 2.7+
 - vagrant
 - libvirt

###Refrences
--------------
- [helm installation](https://helm.sh/docs/intro/install/)
- [stable/minecraft](https://hub.kubeapps.com/charts/stable/minecraft) <= this is where you will find the minecraft variables and values.yaml options
- [Kubeapps](https://github.com/kubeapps/kubeapps/blob/master/docs/user/getting-started.md) <= just run the

###Dependencies
------------


###Usage
-----

Run the playbook as normal by installing requirements:

`ansible-galaxy install -f -r requirements.yml`  

Then calling the playbook itself:

`ansible-playbook letsencrypt-playbook.yml`  

When the playbook completes, you should be able to naviagate to https://<FQDN of server> to login to the Dashboard. Your login credentials will be your Foxpass credentials

###Tags
----



#### Tested Host Operating Systems

- Ubuntu 18.04

#### Tested Target Operating systems

- Ubuntu 18.04

####Known Issues
--------------
