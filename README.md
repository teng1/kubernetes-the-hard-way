# Kubernetes The Hard Way

Following through the GCP tutorial using ansible on local VMs running in Vagrant

## Environment prep

Prepping CoreOS to be managed by ansible takes the same bootstrapping process used in the Kubespray project 

```
# Start CoreOS hosts
Vagrant up 

# Bootstrap CoreOS hosts to correctly run ansible
ansible-playbook site.yaml -i inventory/coreos-lab/hosts.ini --become

```
```
ls -altr | grep key | cut -d ' ' -f9 | sed -s 's/^/ - /' | cut -d '.' -f 1 | sed 's/....$//'
```
