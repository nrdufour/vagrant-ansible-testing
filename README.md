# Vagrant Testing of Ansible Scripts
As we start to build out more and more roles at Blackbaud, we needed an easy way to test them.  Since we maintain servers both in local data centers and in AWS we decided to use Vagrant be able to quickly test our ansible files against both environments.

The master branch includes a Vagrant file that will lanuch instances locally to VMware Fusion and to AWS.  It is loosely based on Jeff Geerling's [ansible-role-test-vms repository] (https://github.com/geerlingguy/ansible-role-test-vms).  The sts branch is geared an Amazon Only environment that utilized role based cross account access via STS.  It includes a shell script (assume-role) that helps setup the environment for STS to work properly.

## Testing a Role
You will need to download a role to you local machine.  This can be done through Ansible Galaxy with the command:
    `$ ansible-galaxy install [rolename]`
    
You will then need to add the role to the playbook.yml file and then run the command `vagrant up`.  This will launch all of the instances defined and run the ansible against it.  

After you have tested the role, you can destroy the instances wiht `vagrant destroy -f`.  You can also just build one instance by identifying it by name in the vagrant up command: `vagrant up ubuntu`.

## License

MIT

## Author Information

Created in 2016 by [Mark Honomichl](http://marsdominion.com/).