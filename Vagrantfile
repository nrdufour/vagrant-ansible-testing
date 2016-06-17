# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.synced_folder '.', '/vagrant', :disabled => true

  # Amazon Linux
  config.vm.define "aws-linux" do |awslinux|
    awslinux.vm.hostname = "awstest"
    awslinux.vm.box = "marsdominion/aws"

    awslinux.vm.provider :aws do |aws, override|
      aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
      aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
      aws.keypair_name = ENV['AWS_KEYPAIR_NAME']
      aws.ami = "ami-8fcee4e5"
      aws.region = "us-east-1"
      aws.instance_type = "t2.micro"
      aws.security_groups = ["ssh-only"]
      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = ENV['MY_PRIVATE_AWS_SSH_KEY_PATH']
      aws.tags = {
        'Name' => 'vagrant-test'
      }
    end

    # Ansible.
    awslinux.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end

# CentOS 7
  config.vm.define "centos7" do |centos7|
    centos7.vm.hostname = "centos7test"
    centos7.vm.box = "marsdominion/centos-7"
    centos7.vm.provider :vmware_fusion do |v, override|
      v.vmx["memsize"] = 1024
      v.vmx["numvcpus"] = 3
    end
    # Ansible.
    centos7.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end

  # ubuntu
  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.hostname = "ubuntutest"
    ubuntu.vm.box = "marsdominion/ubuntu-14.04"

    ubuntu.vm.provider :vmware_fusion do |v, override|
      v.vmx["memsize"] = 1024
      v.vmx["numvcpus"] = 3
    end

    # Ansible.
    ubuntu.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end

end
