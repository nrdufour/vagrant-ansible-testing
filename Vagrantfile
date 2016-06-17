# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.synced_folder '.', '/vagrant', :disabled => true

  # Amazon Linux
  config.vm.define "aws-linux" do |awslinux|
    awslinux.vm.hostname = "awstest"
    awslinux.vm.box = "aws"

    awslinux.vm.provider :aws do |aws, override|
      aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
      aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
      aws.session_token = ENV['AWS_SECURITY_TOKEN']
      aws.keypair_name = ENV['AWS_KEYPAIR_NAME']
      aws.subnet_id = "subnet-667f2e10"
      aws.ami = "ami-8fcee4e5"
      aws.region = "us-east-1"
      aws.instance_type = "t2.micro"
      aws.security_groups = ["sg-030c8b78"]
      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = ENV['MY_PRIVATE_AWS_SSH_KEY_PATH']
      aws.ssh_host_attribute = :private_ip_address
      aws.tags = {
        'Name' => 'vagrant-awslinux'
      }
    end

    # Ansible.
    awslinux.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end

    # Amazon Red Hat
  config.vm.define "aws-redhat" do |awsredhat|
    awsredhat.vm.hostname = "awstest"
    awsredhat.vm.box = "aws"

    awsredhat.vm.provider :aws do |aws, override|
      aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
      aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
      aws.session_token = ENV['AWS_SECURITY_TOKEN']
      aws.keypair_name = ENV['AWS_KEYPAIR_NAME']
      aws.subnet_id = "subnet-667f2e10"
      aws.ami = "ami-8fcee4e5"
      aws.region = "us-east-1"
      aws.instance_type = "t2.micro"
      aws.security_groups = ["sg-030c8b78"]
      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = ENV['MY_PRIVATE_AWS_SSH_KEY_PATH']
      aws.ssh_host_attribute = :private_ip_address
      aws.tags = {
        'Name' => 'vagrant-redhat'
      }
    end

    # Ansible.
    awsredhat.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end

    # Amazon Ubuntu
  config.vm.define "aws-ubuntu" do |awsubuntu|
    awsubuntu.vm.hostname = "awstest"
    awsubuntu.vm.box = "aws"

    awsubuntu.vm.provider :aws do |aws, override|
      aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
      aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
      aws.session_token = ENV['AWS_SECURITY_TOKEN']
      aws.keypair_name = ENV['AWS_KEYPAIR_NAME']
      aws.subnet_id = "subnet-667f2e10"
      aws.ami = "ami-8fcee4e5"
      aws.region = "us-east-1"
      aws.instance_type = "t2.micro"
      aws.security_groups = ["sg-030c8b78"]
      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = ENV['MY_PRIVATE_AWS_SSH_KEY_PATH']
      aws.ssh_host_attribute = :private_ip_address
      aws.tags = {
        'Name' => 'vagrant-ubuntu'
      }
    end

    # Ansible.
    awsubuntu.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end

end
