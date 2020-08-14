# RHEL 8 Ansible Workshop class build out VM

Vagrantfile with buildout configuration for a Virtualbox VM with RHEL 8 server-desktop and configuration for building out an ansible workshop within AWS

## Requirements
1. [VirtualBox & Extension Pack](https://www.virtualbox.org/)
1. [Vagrant](https://www.vagrantup.com/)
1. vagrant-vbguest
    ```sh
    vagrant plugin install vagrant-vbguest 
    ```
1. To register with subscription-manager a free RedHat Developer subscription is required. This Vagrantfile expects to find the credentials in environment variables called `RH_SUBSCRIPTION_MANAGER_USER` and `RH_SUBSCRIPTION_MANAGER_PW`. Ensure these are exported and available to Vagrant, the Vagrantfile will abort if these are not set. 
1. group_var/all.yml file filled in with all your specific settings an example is available [here](https://github.com/RedHatGov/redhatgov.workshops/blob/master/ansible_tower_aws/group_vars/all_example.yml)

## Start
Be sure to update the source_all variable in Vagrantfile of where your all.yml file is located on host system.
To start the vm just run command
```sh
vagrant up
```

## Vagrantfile for RHEL 8

Vagrantfile to spin up a RHEL 8 VM and register with RHN via subscription-manager. It will install the environment for running ansible playbooks to build the red hat ansible workshop on AWS.


## Ansible Workshop build out on AWS
```sh
ansible-playbook 1_provision.yml
```

>__NOTE:__ If 1_provision.yml playbook has errors you will need to start over after running 
>```sh
>ansible-playbook 3_unregister.yml -e NOSSH=true
>```

```sh
ansible-playbook 2_load.yml
```
>__NOTE:__  Playbook 2_load.yml will hang twice while doing the subscription manager tasks. After each time it hangs just run the playbook again.

## To test workshop
copy the test-workshop.yml file onto the admin server and run
```sh
ansible nodes -m shell -a 'rpm -qa | grep docker'
ansible nodes -m shell -a 'rpm -qa | grep podman'
ansible nodes -m shell -a 'docker images redhatgov/alpine' -b 
ansible nodes -m shell -a 'docker images redhatgov/fedora' -b
ansible nodes -m shell -a 'podman images redhatgov/alpine' -b 
ansible nodes -m shell -a 'podman images redhatgov/fedora' -b
```

## To remove the workshop environment from AWS
```sh
ansible-playbook 3_unregister.yml
```
> __NOTE:__ If any errors while unregistering run again with environment variable
>```sh
>ansible-playbook 3_unregister.yml -e NOSSH=true
>```

## Notes

Using the current latest versions of Vagrant and VirtualBox on MacOS, the version of VirtualBox Guest Additions is newer than the version packaged in roboxes/rhel8. Vagrant will try and update this before the VM has been registered with RHN so all calls to yum install fail. For this reason `config.vbguest.auto_update = false` is configured.