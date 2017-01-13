# -*- mode: ruby -*-
# vi: set ft=ruby :

# Source the Config File if it exists

if File.exists?('env.rb')
  require_relative "./env.rb"
else
  puts "File doesn't exist"
end

# If the vagrant command is up or provision, rerun the requirements to pickup changes
system("
    if [ #{ARGV[0]} = 'up' ] || [ #{ARGV[0]} = 'provision' ]; then
      ansible-galaxy install -f -r requirements.yml
    fi
")

aws_access_key = ENV['AWS_ACCESS_KEY_ID']
aws_secret_key = ENV['AWS_SECRET_ACCESS_KEY']
aws_keypair = ENV['AWS_KEYPAIR_NAME']
private_key_path = ENV['MY_PRIVATE_AWS_SSH_KEY_PATH']
aws_subnet = ENV['AWS_SUBNET']
aws_sg = ENV['AWS_SG']

# Optional Variables
aws_region = ENV['AWS_DEFAULT_REGION'] || 'us-east-1'
aws_instance = ENV['AWS_INSTANCE_TYPE'] || 't2.micro'
aws_ami = ENV['AWS_AMI'] || 'ami-9be6f38c'
aws_ec2_user = ENV['AWS_EC2_USER'] || 'ec2-user'

if aws_access_key.nil?
  print "AWS_ACCESS_KEY_ID is not defined\n"
  exit(1)
end

if aws_secret_key.nil?
  print "AWS_SECRET_ACCESS_KEY is not defined\n"
  exit(1)
end

if aws_keypair.nil?
  print "AWS_KEYPAIR_NAME is not defined\n"
  exit(1)
end

if private_key_path.nil?
  print "MY_PRIVATE_AWS_SSH_KEY_PATH is not defined\n"
  exit(1)
end

if aws_subnet.nil?
  print "AWS_SUBNET is not defined\n"
  exit(1)
end

if aws_sg.nil?
  print "AWS_SG is not defined\n"
  exit(1)
end

Vagrant.configure(2) do |config|
  config.vm.synced_folder '.', '/vagrant', :disabled => true

  # Amazon Linux
  config.vm.define "aws-linux" do |awslinux|
    awslinux.vm.hostname = "awstest"
    awslinux.vm.box = "marsdominion/aws"

    awslinux.vm.provider :aws do |aws, override|
      aws.access_key_id = aws_access_key
      aws.secret_access_key = aws_secret_key
      aws.keypair_name = aws_keypair
      aws.ami = aws_ami
      aws.region = aws_region
      aws.instance_type = aws_instance
      aws.subnet_id = aws_subnet
      aws.security_groups = aws_sg
      override.ssh.username = aws_ec2_user
      override.ssh.private_key_path = private_key_path
      aws.ssh_host_attribute = :public_ip_address
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
