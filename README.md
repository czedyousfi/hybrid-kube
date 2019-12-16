***This is a WORK IN PROGRESS proof of concept, only for testing purposes***

Inspired by those labs, but with few modifications : 

https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/
https://codelabs.developers.google.com/codelabs/istio-multi-burst/#15
https://cloud.google.com/community/tutorials/using-cloud-vpn-with-strongswan?hl=fr

*Directories :*
* /cluster-onprem : 
  * /kubernetes-setup : those files serve the kubernetes on-premise cluster on vagrant. It will initiate 3 nodes for kubernetes cluster that are completely operationals 
  * /strongSwan : a work-in-progress VPN server based on strongSwan software. 
  * /kubernetes : yml files to deploy an application on kubernetes. 
 
 To run the cluster, make sure you have vagrant and Virtual Box install and run : `vagrant up` 
 then you'll be able to `vagrant ssh`

*
*
