# -*- mode: ruby -*-
# vi: set ft=ruby :

require "json"

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

# Define the environmental variables and check to see if they exist
role_arn = ENV['ROLE_ARN']
aws_keypair = ENV['AWS_KEYPAIR_NAME']
private_key_path = ENV['MY_PRIVATE_AWS_SSH_KEY_PATH']
aws_subnet = ENV['AWS_SUBNET']
aws_sg = ENV['AWS_SG']

# Optional Variables
aws_region = ENV['AWS_DEFAULT_REGION'] || 'us-east-1'
aws_instance = ENV['AWS_INSTANCE_TYPE'] || 't2.micro'
aws_ami = ENV['AWS_AMI'] || 'ami-9be6f38c'
aws_ec2_user = ENV['AWS_EC2_USER'] || 'ec2-user'


if role_arn.nil?
  print "ROLE_ARN is not defined\n"
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


# Get the Credentials
credstore = JSON.parse(`aws sts assume-role --role-arn "${ROLE_ARN}" --role-session-name "vagrant" --query 'Credentials' --output json`)



Vagrant.configure(2) do |config|
  config.vm.synced_folder '.', '/vagrant', :disabled => true

  # Amazon Linux
  config.vm.define "aws-linux" do |awslinux|
    awslinux.vm.hostname = "ansible-test-node"
    awslinux.vm.box = "marsdominion/aws"

    awslinux.vm.provider :aws do |aws, override|
      aws.access_key_id = credstore["AccessKeyId"]
      aws.secret_access_key = credstore["SecretAccessKey"]
      aws.session_token = credstore["SessionToken"]
      aws.keypair_name = aws_keypair
      aws.subnet_id = aws_subnet
      aws.ami = aws_ami
      aws.region = aws_region
      aws.instance_type = aws_instance
      aws.security_groups = aws_sg
      override.ssh.username = aws_ec2_user
      override.ssh.private_key_path = private_key_path
      aws.ssh_host_attribute = :private_ip_address
      aws.tags = {
        'Name' => 'Ansible Test Server'
      }
    end

    # Ansible.
    awslinux.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvv"
      ansible.playbook = "playbook.yml"
    end
  end
end