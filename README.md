# Vagrant Testing of Ansible Scripts
As we start to build out more and more roles at Blackbaud, we needed an easy way to test them.  Since we maintain servers both in local data centers and in AWS we decided to use Vagrant be able to quickly test our ansible files against both environments.

The master branch includes a Vagrant file that will lanuch instances locally to VMware Fusion and to AWS.  It is loosely based on Jeff Geerling's [ansible-role-test-vms repository] (https://github.com/geerlingguy/ansible-role-test-vms).  The sts branch is geared towards an Amazon Only environment that utilizes role based cross account access via STS.  It includes a shell script (assume-role) that helps setup the environment for STS to work properly.

## Testing a Role
Before Setting it up, you will need to define some environmental variable for the AWS portion to work correctly.  In Master, you will need to set:

      AWS_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY
      AWS_KEYPAIR_NAME
      MY_PRIVATE_AWS_SSH_KEY_PATH
      
In STS, you will also need to set: 

      AWS_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY
      AWS_SECURITY_TOKEN
      AWS_KEYPAIR_NAME
      AWS_SUBNET_ID
      AWS_SECURITY_GROUP_ID
      MY_PRIVATE_AWS_SSH_KEY_PATH

In the sts branch, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and AWS_SECURITY_TOKEN are set by the assume_role script if you use it.  We also have to define the Subnet ID and Security Group ID in AWS because we delete the default VPC.  I've also added the parameter to to use the private IP address since I don't generally setup production addresses on test instances.

You will need to download a role to you local machine.  This can be done through Ansible Galaxy with the command:
    `$ ansible-galaxy install [rolename]`
    
You will then need to add the role to the playbook.yml file and then run the command `vagrant up`.  This will launch all of the instances defined and run the ansible against it.  

After you have tested the role, you can destroy the instances wiht `vagrant destroy -f`.  You can also just build one instance by identifying it by name in the vagrant up command: `vagrant up ubuntu`.

## License

MIT

## Author Information

Created in 2016 by [Mark Honomichl](http://marsdominion.com/).