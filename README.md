# Vagrant Testing of Ansible Scripts
As we start to build out more and more roles at Blackbaud, we needed an easy way to test them.  Since we maintain servers both in local data centers and in AWS we decided to use Vagrant be able to quickly test our ansible files against both environments.

The master branch includes a Vagrant file that will lanuch instances locally to VMware Fusion and to AWS.  It is loosely based on Jeff Geerling's [ansible-role-test-vms repository] (https://github.com/geerlingguy/ansible-role-test-vms).  The sts branch is geared towards an Amazon Only environment that utilizes role based cross account access via STS.

## Testing a Role
Before Setting it up, you will need to define some environmental variable for the AWS portion to work correctly.  In Master, you will need to set:

      AWS_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY
      AWS_KEYPAIR_NAME
      MY_PRIVATE_AWS_SSH_KEY_PATH
      
In STS, the easiest thing to do is setup an env.rb file in your local directory, and add the following lines to it:

    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    
    ENV['AWS_PROFILE'] = 'my-aws-profile'
    ENV['ROLE_ARN'] = 'arn:aws:iam::XXXXXXXXXXXX:role/platform-engineering'
    ENV['AWS_KEYPAIR_NAME'] = 'my-keypair'
    ENV['MY_PRIVATE_AWS_SSH_KEY_PATH'] = '/Users/me/.ssh/my-keypair'
    ENV['AWS_SUBNET'] = 'subnet-xxxxxxxx'
    ENV['AWS_SG'] = 'sg-xxxxxxxx'
    
You will then need to add the role to the playbook.yml file and then run the command `vagrant up`.  This will launch all of the instances defined and run the ansible against it.  

After you have tested the role, you can destroy the instances wiht `vagrant destroy -f`.  You can also just build one instance by identifying it by name in the vagrant up command: `vagrant up ubuntu`.

## License

MIT

## Author Information

Created in 2016 by [Mark Honomichl](http://marsdominion.com/).